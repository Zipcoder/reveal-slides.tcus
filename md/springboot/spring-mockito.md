#Testing Services and Controllers: 

##Intro to Mockito

-
-
## Reminder: Why we test

* To ensure modules work to achieve desired functionality
* To validate new code does not break old functionality
* To allow multiple developers to quickly validate code

-

##Integration Testing

* A style of testing by which we test multiple modules together to show functionality in a service

* *It is not a replacement for unit testing*

* Allows for testing in a running environment rather than in an isolated system.

-
-

## Testing in Spring Boot

To test a Spring Boot module, you will often times rely on Spring Boot libraries. To load these, you would have to load the Spring Application Context. We can do so by using `@SpringBootTest`


-
## Example Test

```Java
@SpringBootTest
@RunWith(SpringRunner.class)
public class ApplicationContextTests {

 @Autowired
 private ApplicationContext applicationContext;

 @Test
 public void contextLoads() throws Throwable {
  Assert.assertNotNull(this.applicationContext);
  // The Application Context is autowired into
  // the test without having to declare it in
  // an @Before method. This is because Spring
  // has been loaded and will be able to Autowire
 }
}
```

-
## Testing in Spring Boot

The @RunWith annotation is specific to the JUnit framework. The @RunWith annotation tells JUnit which test runner strategy to use. 

In this case, we want to run our test with the Spring TestContext Framework, a module in the Spring Framework that provides generic test support for Spring applications.

-
-

## Mocking in Tests

Often times we want a test to run without having to rely on another class working. This could be because:

* The class our module depends on is still in development
* The class our module depends on is hitting a backing service
* The class our module depends on is not being loaded in the scope of testing

-

#Mocking in Tests


Components often depend on accessing external systems, when implemented in production.

Because of this, we need a way to isolate tests on the functionality of features without needing to bring the entire class heirarchy for the tests to work. 

We can insert Mockito mocks into Spring Beans for unit testing by using dependency injection, thus allowing us to introduce that isolation.

-

## Mocking in Tests

Regardless of whether or not as class is available to you, it is always better to write tests assuming that dependencies will always function exactly as expected. 

*We are not writing a test for the dependencies of the module*

To ensure that a dependency will work as expected we use a technique called Mocking

-

## Mocking in Tests

Mocking is the method by which we make a fake class to inject into a module. Spring Boot uses Mockito to create mocks of beans called MockBeans



-


## Mocking in Tests

Spring Boot supports the `@MockBean` annotation

@MockBean instructs Spring to include a class in the Application Context that mutes the true implentation. 

 
-

## Mocking in Tests

```Java
  // These services have been mocked
  // Because of this, we may provide
  // return values for the functions
  // rather than allow the functions
  // to run against a real database.
  @MockBean
  private UserService userService;

  @MockBean
  private AccountRepository accountRepository;
```

-

## Mocking in Tests

```Java
  @Before
  public void before() {
    // Here we will test the AccountService class.
    // We pass in the mocked classes so that we
    // don't have to rely that their implementation
    // is correct
    accountService = new AccountService(
      accountRepository, userService);
 }
```

-

## Mocking in Tests

```Java
 @Test
 public void getUserAccountReturnsAccount() throws Exception {
  // We have a given clause where we pass in
  // what a function will return when called
  given(
    this.accountRepository
    .findAccountByUsername("user")
  )
  .willReturn(
      // We know now that when the above function
      // is called the below content will be
      // returned to us
      new Account("user", new AccountNumber("123456789"))
  );

```

-

## Mocking in Tests

```Java
  // The getUserAccounts will call the method
  // accountRepository.findAccountsByUsername("user")
  // Because we know what that will return we
  // can predict what to expect and write an
  // assertion to meet that expectation
  Account actual = accountService.getUserAccount("user");
```
-

## Mocking in Tests

```Java
  // Since we know the account repository will return 
  // the user we supplied and our account service is 
  // supposed to return whatever the repository returns
  // we can make an effective test
  assertThat(actual)
    .isEqualTo(
      new Account("user", new AccountNumber("123456789"))
    );

```

-

## Slices

In Spring, a Slice is a piece of the Spring Boot Application Context. Often times when running a test, you may only need specific parts of your module to be loaded.

Loading only necessary modules will allow your tests to run faster and be more useful for development

Spring Boot provides multiple testing annotations that target a specific slice of your application.

-

## @JSONTEST

The @JsonTest annotation allows you to activate just the configuration to test JSON serialization and deserialization.

-

## @JSONTEST

```Java
  // This class is loaded because it is
  // part of the configuration resposible
  // for serialization
  @Autowired
  private JacksonTester<User> json;
  ...
  // we may run methods of the JacksonTester module
  // and expect content even though other parts of
  // the Spring Application Context aren't loaded 
  assertThat(this.json.write(user))
    .isEqualTo("user.json");
  assertThat(this.json.write(user))
    .isEqualToJson("user.json");
  assertThat(this.json.write(user))
    .hasJsonPathStringValue("@.username");

```
You may further look into Json testing in Spring by looking up AssertJ, a JSON tester backed by Jackson

-

## @WEBMVCTEST

The `@WebMvcTest` annotation supports testing individual Spring MVC controllers

Only modules needed to run controller methods will be loaded

-
## @WEBMVCTEST

```Java
  // This class is loaded in the WebMVC context
  // and can be autowired
  @Autowired
  private MockMvc mvc;

  // This service needs to be mocked for our
  // controller to function
  @MockBean
  private AccountService accountService;
```
-
## @WEBMVCTEST

```Java
  @Test
  public void getUserAccountReturnsAccount() throws Exception {
    // this will be what we expect our api to return
    String expected = "[{\"username\": \"user\", \"accountNumber\": \"123456789\"}]";
  
    // here we supply what our accountService will
    // return a particular account. Our Controller
    // should return the json representaiton of that
    given(
      this.accountService.getUserAccount(1)
    )
    .willReturn(
      new Account("user", "123456789")
    );
  ```
-

## @WEBMVCTEST

```Java
    // we use mvc to call whatever method is mapped
    // to /v1/accounts. This route should be mapped
    // to the controller we are testing
    this.mvc.perform(
      get("/v1/accounts/1").accept(MediaType.APPLICATION_JSON)
    )
    // The status should be ok and the returned json
    // should be equal to the json representation of
    // the object we passed into our mock service
    .andExpect(status().isOk())
    .andExpect(content().json(expected));
  }
```

-
-

## The Structure of a Test

No matter what slice we use the structure will usually be the same. 

1. Mock any dependencies and supply mock returns
2. Invoke the method we are testing
3. Write assertions based on the behavior we expect


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
<img src="./img/bunnies/springbunny.jpg">