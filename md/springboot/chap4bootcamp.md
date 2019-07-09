# Spring Boot Testing

-
## Need for tests

* Ensure modules work to achieve desired functionality
* Validate new code does not break old functionality
* Allow multiple developers to quickly validate code

-

## Integration Testing

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

## Mocking in Tests

Reglardless of whether or not as class is available to you, it is always better to write tests assuming that dependencies will always function exactly as expected. 

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
