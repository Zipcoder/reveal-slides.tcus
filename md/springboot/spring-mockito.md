#Testing Services and Controllers: 

##Intro to Mockito

-
-
#Mocks

Components often depend on accessing external systems, when implemented in production.

Because of this, we need a way to isolate tests on the functionality of features without needing to bring the entire class heirarchy for the tests to work. 

We can insert Mockito mocks into Spring Beans for unit testing by using dependency injection, thus allowing us to introduce that isolation.



-
-

## Maven Dependencies for your `pom.xml`

Using a version of our **CD** components from the JPA exercise as an example, we'll add the dependencies to the `pom.xml`:

```xml
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>2.24.0</version>
        </dependency>
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-junit-jupiter</artifactId>
            <scope>test</scope>
        </dependency>
```

-
-

## Our CD Classes

For these examples, we're using the a CD classes:

- CD
- CDRepo
- CDService
- CDController
 

-
### Our CD Classes (continued)
Our **CD** class, in brief: 
```Java
@Entity
public class CD {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    Long id;
    String title;
    String description;
    Integer year;
    Float price;

    public CD(){}
    
    public CD(String title) {
        this.title = title;
    }
    
    //methods omitted for brevity
}
```
-
### Our CD Classes (continued)

Our **CDRepo** interface, in brief: 

```Java
@Repository
public interface CDRepo extends JpaRepository<CD, Long> {
    CD findCDById(Long id);

    CD findCDByTitle(String title);

    List<CD> findAll();
}
```


-
### Our CD Classes (continued)

Our **CDService** class, in brief: 

```Java
@Service
public class CDService {
    @Autowired
    private CDRepo cdRepo;
    
    public CD saveCD(CD cd) {
        cdRepo.save(cd);
        return cd;
    }
    public CD findCD(Long id) {
        return cdRepo.findCDById(id);
    }
    public CD findCDByTitle(String title) {
        return cdRepo.findCDByTitle(title);
    }
    public List<CD> findAllCds() {
        return cdRepo.findAll();
    }
}

```


-
### Our CD Classes (continued)

Our **CDController** class, in brief: 

```Java
@RestController
public class CDController {
    @Autowired
    CDService cdService;
    
    @GetMapping("/cd/all")
    public @ResponseBody List<CD> findAllCds(){
        return cdService.findAllCds();
    }
    
    @PostMapping("/cd/create")
    public @ResponseBody
    CD createCD(@RequestBody CD cd) {
        cdService.saveCD(cd);
        return cd;
    }
    
    @PutMapping("/cd/update")
    public @ResponseBody CD updateCD(@RequestBody CD cd) {
        cdService.saveCD(cd);
        return cd;
    }
}
```

-
-

## Testing a Service

Now that we have our classes, we'll configure the application context we need for the tests. Note the `@Profile` annotation, designating the context as **"test"** for the purpose of our unit tests. The primary bean used for our mock will be **cdService**.

```Java
@Profile("test")
@Configuration
public class CDServiceTestConfiguration {
    @Bean
    @Primary
    public CDService cdService() {
        return Mockito.mock(CDService.class);
    }
}

```
-

### Testing a Service, continued:

Now, for our unit tests, we'll also designate the application context as **"test"**, tell **SpringBootTest** to run our main application class, and auto-wire the service we just mocked: 

```Java
@ActiveProfiles("test")
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = MocksDemoApplication.class)
public class CDServiceUnitTest {
    @Autowired
    private CDService cdService;
    
    //omitted rest for brevity
}
```

-

### Testing a Service, continued:

Now, we can test our service by establishing the condition of **"when"** and **"then"**, telegraphing the expected behavior of our service according to the conditions prescribed. 

In this case, it's: "when **findCDByTitle** is called, **thenReturn** the **CD** that has the expected **title**."

```Java
@Test
public void whenTitleIsProvided_thenRetrievedTitleIsCorrect() {
    String mockTitle = "My New Album";
    String expectedTitle = "My New Album";
    Mockito.when(cdService.findCDByTitle(mockTitle)).thenReturn(new CD(mockTitle));
    
    String testName = cdService.findCDByTitle(mockTitle).getTitle();
    
    Assert.assertEquals(expectedTitle, testName);
}

```
-
-
## Testing a Controller

Now, we'll write a test for our CDController. 

-

### Testing a Controller (continued)

For this test, we'll establish **"test"** as our application context, tell **WebMvcTest** to use our **CDController** class. We'll also auto-wire **MockMvc** and call our **MockBean** for **cdController**. 
```Java
@WebMvcTest(controllers = CDController.class)
@ActiveProfiles("test")
public class CDControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private CDController cdController;

    //omitted for brevity
}

```
-
### Testing a Controller (continued)


Next, we'll set up the content we're going to test with, first by declaring a list of CDs, and populating the list for a controller response. This will run before our test runs:

```Java
    private List<CD> cdList;
    @Before
    public void setUp() {
        //initialize the controller mock and build:
        cdController = Mockito.mock(CDController.class);
        mockMvc = MockMvcBuilders.standaloneSetup(cdController).build();
        this.cdList = new ArrayList<>();
        this.cdList.add(new CD("Official Soundtrack to a Work in Progress"));
        this.cdList.add(new CD("Aural Impetus"));
        this.cdList.add(new CD("No Time For Enemies"));
    }
```


-
### Testing a Controller (continued)


Now we'll test our **"/cd/all"** endpoint by verifying that it returns all the CDs we populated our mock with, first by setting up our expected behavior: "**when** findAllCDs() is called, **thenReturn** the expected list of CDs" 

```Java
@Test
public void shouldFindAllCdsTest() throws Exception {
    Mockito.when(cdController.findAllCds()).thenReturn(cdList);
    
    mockMvc.perform(get("/cd/all")).andExpect(status().isOk()).andExpect(jsonPath("$.size()", is(cdList.size())));
}
```


-
### Testing a Controller (continued)
 
 Next, we perform the `GET` request to the endpoint, asserting the status **isOk** (200), and the JSON response contains the **cdList**. Here, we compare the size of the response to the size of **cdList**.



```Java
@Test
public void shouldFindAllCdsTest() throws Exception {
    Mockito.when(cdController.findAllCds()).thenReturn(cdList);
    mockMvc.perform(get("/cd/all")).andExpect(status().isOk()).andExpect(jsonPath("$.size()", is(cdList.size())));
}

```

-
-
#Full reference documentation: 

<a href="https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html" target="_blank">https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html</a>

-
-
<img src="/img/bunnies/springbunny.jpg">