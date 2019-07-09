#Introducing Spring Boot and Cloud Foundry

-
-

# What Is Spring Boot
When we say an application is `cloud native` it means that it is designed to thrive in a cloud based enviorment.

`Spring Boot` provides a way to create production-ready Spring applications with minimal setup time.

-

#What is Spring Boot

* An opinionated, production ready way to consume all of Spring  
* Provides meaningful defaults and automatic configurations
* Support for embedded HTTP servers

Think of it like 

`Spring Boot = (Spring + Tomcat) - (@Configurations)`

-

#An opinionated view

An `opinionated view` means that Spring Boot lays out a set of familiar abstractions that are common to all Spring projects.

making it simple to swap components when project requirements change.

Spring Boot handles conflicting transitive dependencies, Meaning that if something changes in your application like it's location your dependencies will still compile.

-

#What is Spring Boot

The main point of spring boot is to simply to allow users to get up and running quickly with Spring.

-
-

# Getting Started with the Spring Initializr
Start spring projects with Spring Boot

1. [http://start.spring.io](http://start.spring.io)
2. `Intellij` file -> new -> spring starter project

-

# Select Dependencies
Select Dependencies and generate project. Then open generated pom file as a project in your ide.
<img src="img/springstarter.png" alt="codebase" width="50%"/>

-
-

# A rudimentary Spring Boot application with a JPA entity Cat

-

# Inside the pom
```
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
        .
        .
	</dependencies>
```
-
# spring-boot-starter-data-jpa
brings in everything needed to persist Java objects using the Java ORM (Object Relational Mapping) specification and JPA (the Java Persistence API) and be productive out of the gate.

This includes the JPA specification types, basic SQL Java database connectivity (JDBC) and JPA support for Spring, Hibernate as an implementation, Spring Data JPA, and Spring Data REST.

-

```
	<dependencies>
		.
        .
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-rest</artifactId>
		</dependency>
        .
        .
	</dependencies>
```
-

# spring-boot-starter-data-rest
makes it trivial to export hypermedia-aware REST services from a Spring Data repository definition.

-

```
	<dependencies>
		.
        .
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
        .
        .
	</dependencies>
```
-

# spring-boot-starter-web
brings in everything needed to build REST applications with Spring. It brings in JSON and XML marshalling support, file upload support, an embedded web container (the default is the latest version of Apache Tomcat), validation support, the Servlet API, and so much more.

-

```
	<dependencies>
		.
        .
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>
        .
        .
	</dependencies>
```
-

# h2
h2 is an in-memory, embedded SQL database. If Spring Boot detects an embedded database like H2, Derby, or HSQL on the classpath and it detects that you haven’t otherwise configured a javax.sql.DataSource somewhere, it’ll configure one for you.

-

```
	<dependencies>
		.
        .
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```
-

# spring-boot-starter-test
brings in all the default types needed to write effective mock and integration tests, including the Spring MVC test framework. It’s assumed that testing support is needed, and this dependency is added by default.

-
-

# Inside Spring Boot application with a JPA entity Cat

-

# @SpringBootApplication
Annotates a class as a Spring Boot application.
```
@SpringBootApplication
public class DemoApplication {

```

-

# SpringApplication.run()
Starts the Spring Boot application.
```
public static void main(String[] args) {
  SpringApplication.run(DemoApplication.class, args);
 }

```

-

# A plain JPA entity to model a Cat entity.
```
@Entity
class Cat {

```

-

# A Spring Data JPA repository 
```
@RepositoryRestResource
interface CatRepository extends JpaRepository<Cat, Long> {
}
```

-
-
# An integration test for our application

-

This is a unit test that leverages the Spring framework test runner. We configure it to also interact nicely with the Spring Boot testing apparatus, standing up a mock web application.

-
```
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
public class DemoApplicationTests {
```
-
```
//Inject a Spring MVC test MockMvc client with which we make calls to the REST endpoints.
 @Autowired
 private MockMvc mvc;
```
-

```
//We can reference any other beans in our 
//Spring application context, including CatRepository.
@Autowired
 private CatRepository catRepository;
```

-

```
//Install some sample data in the database.
@Before
 public void before() throws Exception {
  Stream.of("Felix", "Garfield", "Whiskers").forEach(
   n -> catRepository.save(new Cat(n)));
 }
```

-

# Invoke the HTTP GET endpoint for the /cats resource.

-

```
 @Test
 public void catsReflectedInRead() throws Exception {
  MediaType halJson = MediaType
   .parseMediaType("application/hal+json;charset=UTF-8");
  this.mvc
   .perform(get("/cats"))
   .andExpect(status().isOk())
   .andExpect(content().contentType(halJson))
   .andExpect(
    mvcResult -> {
     String contentAsString = mvcResult.getResponse().getContentAsString();
     assertTrue(contentAsString.split("totalElements")[1].split(":")[1].trim()
      .split(",")[0].equals("3"));
    });
 }
```
-

 Some example Spring Boot Starters
<img src="img/bootstarters.png" alt="codebase" width="50%"/>

-
-

# The Spring Guides
The Spring guides are a set of small, focused introductions to all manner of different topics in terms of Spring projects.

[https://spring.io/guides](https://spring.io/guides) 

Each guide features a GitHub repository that contains three folders: complete, initial, and test. 
The test folder has whatever’s required to confirm that the complete project works.

-
-

# Configuration
 A Spring application is a collection of objects, as the case with java. Spring refers to your objects as `beans`.

 Spring manages them and their relationships for you.

 In order for Spring to support your beans it needs to be made aware of them.

-

 # Configuration
 Let’s suppose we have a typical layered service, 
 with a service object in turn talking to a javax.sql.DataSource to talk to an (in this case) embedded database, H2.

 We could instantiate that datasource in place, where it’s needed, but then we’d have to duplicate that logic whenever we wish to reuse the datasource in other objects.

 we will look at an example in our customer service class

-

 **we can’t reuse that logic elsewhere**

-

#### class CustomerService
 ```
 ...
//It’s hard to stage the datasource, 
//as the only reference to it is buried in a private 
//final field in the CustomerService class itself. 
//The only way to get access to that variable is
// by taking advantage of Java’s friend access,
//where instances of a given object are able to see other instances,
//private variables.
  DataSource dataSource = customerService.dataSource;
  DataSourceInitializer init = new DataSourceInitializer();
  init.setDataSource(dataSource);
  ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
  populator.setScripts(new ClassPathResource("schema.sql"),
   new ClassPathResource("data.sql"));
  init.setDatabasePopulator(populator);
  init.afterPropertiesSet();
 ```

-

 ```
//Here we use types from Spring itself,
//not JUnit since the JUnit library is test scoped,
//to “exercise” the component. 
//The Spring framework Assert class supports,
//design-by-contract behavior, 
//not unit testing!
 int size = customerService.findAll().size();
  Assert.isTrue(size == 2);
 ```

-

 **we can’t plug in a mock datasource, and we can’t stage the datasource any other way, we’re forced to embed the test code in the component itself.**

 It would be cleaner to centralize the bean definition, outside of the call sites where it’s used.

 we could create the objects and establish their wiring in one place: in a single class. This principle is called `Inversion of Control (IoC)`.

-
The wiring of objects is separate from the components themselves. In separating these things apart, we’re able to build component code that is dependent on base-types and interfaces, not coupled to a particular implementation.

this is called `dependency injection`

-

Now let’s move the wiring of the objects and the configuration into a separate class: a configuration class.

-

```
//This class is a Spring @Configuration class,
//which tells Spring that it can expect to find-
//definitions of objects and how they wire together in this class.
@Configuration
public class ApplicationConfiguration {
```

-

We’ll extract the definition of the DataSource into a bean definition. Any other Spring component can see, and work with, this single instance of the DataSource.

Any Spring components that depend on the DataSource will have access to the same instance in memory.

This has to do with Spring’s notion of scope. By default, a Spring bean is singleton scoped.

-

```
 @Bean(destroyMethod = "shutdown")
 DataSource dataSource() {
  return new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2)
   .setName("customers").build();
 }

```

-

Register the CustomerService instance with Spring and tell Spring to satisfy the DataSource by sifting through the other registered beans in the application context and finding the one whose type matches the bean provider parameter.

`Remeber the beans are simply your created objects`

-

```
@Bean
 CustomerService customerService(DataSource dataSource) {
  return new CustomerService(dataSource);
 }
```

Now we Revisit the CustomerService and remove the explicit DataSource creation logic 

-

```
 public CustomerService(DataSource dataSource) {
  this.dataSource = dataSource;
 }
```
Instead of an entire config setup in our **CustomerService class**

-
```
DataSource dataSource = customerService.dataSource;
DataSourceInitializer init = new DataSourceInitializer();
init.setDataSource(dataSource);
ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
populator.setScripts(new ClassPathResource("schema.sql"),
new ClassPathResource("data.sql"));
init.setDatabasePopulator(populator);
init.afterPropertiesSet();
 ```

-

 The definition of the CustomerService type is markedly simpler, since it now merely depends on a DataSource. We’ve limited its responsibilities, arguably, to things more in scope for this type: interacting with the dataSource, not defining the dataSource itself.

 The configuration is explicit, but also a bit redundant. Spring could do some of the construction for us, if we let it.

-

```
@Configuration
@ComponentScan //<-----
public class ApplicationConfiguration {

 @Bean(destroyMethod = "shutdown")
 DataSource dataSource() {
  return new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2)
   .setName("customers").build();
 }
}
```
-
This annotation tells Spring to discover other beans in the application context by scanning the current package (or below) and looking for all objects annotated with stereotype annotations like @Component.

Spring perceives them on components and creates a new instance of the object on which they’re applied. It calls the no-argument constructor by default

-

The code in our example uses a datasource directly, and we’re forced to write a lot of low-level boilerplate JDBC code to get a simple result. 

Dependency injection is a powerful tool, but it’s the least interesting aspect of Spring.

-

We will use `portable service abstractions`, to simplify our interaction with the datasource. We’ll swap out our manual and verbose JDBC-code and use Spring framework’s JdbcTemplate.

-

```
 @Bean
 JdbcTemplate jdbcTemplate(DataSource dataSource) {
  return new JdbcTemplate(dataSource);
 }
```
instead of
```
 @Bean
 CustomerService customerService(DataSource dataSource) {
  return new CustomerService(dataSource);
 }
```

-

The JdbcTemplate is one of many implementations in the Spring ecosystem of the `template pattern`.

It provides convenient utility methods that make working with JDBC a one-liner for common things.

`JDBC`: Java Database Connectivity

-

#### A considerably simpler CustomerService
```
private final JdbcTemplate jdbcTemplate;

 public CustomerService(JdbcTemplate jdbcTemplate) {
  this.jdbcTemplate = jdbcTemplate;
 }

 public Collection<Customer> findAll() {
  
  RowMapper<Customer> rowMapper = (rs, i) -> new Customer(rs.getLong("ID"),
   rs.getString("EMAIL"));
  
  return this.jdbcTemplate.query("select * from CUSTOMERS ", rowMapper);
 }
```
-

There are many overloaded variants of the query method, one of which expects a RowMapper implementation. It is a callback object that Spring will invoke for you on each returned result, allowing you to map objects returned from the database to the domain object of your system. The RowMapper interface also lends itself nicely to Java 8 lambdas!

**think of lambdas as carriers they just pass information(returns etc) along**

-

The query is a trivial one-liner now. Which is much better!

-
-

# Aspect-oriented programming (AOP)

As we control the wiring in a central place, in the bean configuration, we’re able to substitute (or inject) implementations with different specializations or capabilities.

If we wanted to, we could change all the injected implementations to support cross-cutting concerns without altering the consumers of the beans.

-

# Aspect-oriented programming (AOP)

Suppose we wanted to support logging the time it takes to invoke all methods. We could create a class that subclasses our existing CustomerService and then, in method overrides, insert logging functionality before and after we invoke the super implementation.

The logging functionality is a cross-cutting concern, but in order to inject it into the behavior of our object hierarchy we’d have to override all methods.

-

Ideally, we wouldn’t need to go through so many hoops to interpose trivial cross-cutting concerns over objects like this.

Languages like Java that only support single-inheritance don’t provide a clean way to address this use case for any arbitrary object.

Spring supports an alternative: aspect-oriented programming (AOP).

-

Spring’s AOP support centers around the notion of an `aspect`, which codifies cross-cutting behavior. 

A `pointcut` describes the pattern that should be matched when applying an aspect.

The pointcut language lets you describe method invocations for objects in a Spring application.

 Let’s suppose we wanted to create an aspect that will match all method invocations, in our CustomerService example and interpose logging to capture timing.

-

 Add ```@EnableAspectJAutoProxy``` to the ApplicationConfiguration ```@Configuration``` class to activate Spring’s AOP functionality. Then, we need only extract our cross-cutting functionality into a separate type, an ```@Aspect``` annotated object 

-

### Extract the cross-cutting concern into a Spring AOP aspect

Mark this bean as an aspect.

```
@Component
@Aspect
public class LoggingAroundAspect {

 private Log log = LogFactory.getLog(getClass());
```
-

Declare that this method is to be given a chance to execute around—before and after—the execution of any method that matches the pointcut expression in the @Around annotation. There are numerous other annotations, but for now this one will give us a lot of power.
-
```
 @Around("execution(* com.example.aop.CustomerService.*(..))")
 public Object log(ProceedingJoinPoint joinPoint) throws Throwable {
  LocalDateTime start = LocalDateTime.now();

  Throwable toThrow = null;
  Object returnValue = null;
```
-

When the method matching this pointcut is invoked, our aspect is invoked first, and passed a ProceedingJoinPoint which is a handle on the running method invocation. We can choose to interrogate the method execution, to proceed with it, to skip it, etc. 

-

```
try {
   returnValue = joinPoint.proceed();
  }
  catch (Throwable t) {
   toThrow = t;
  }
  LocalDateTime stop = LocalDateTime.now();

  log.info("starting @ " + start.toString());
  log.info("finishing @ " + stop.toString() + " with duration "
   + stop.minusNanos(start.getNano()).getNano());
```

-

If an exception is thrown, it is cached and rethrown later.

```
if (null != toThrow)
   throw toThrow;
```
-

If a return value is recorded, it is returned as well (assuming no exception’s been thrown).

```
 return returnValue;
 }
}
```

-

If we were to introduce another business service method—one that mutated the database multiple times in a single service call—then we’d need to ensure that those mutations all happened in a single unit of work; either every interaction with the stateful resource (the datasource) succeeds, or none of them do.

-

Spring takes care of these issues and most of our config needs

-
e.g if We wanted to build a Spring Boot version of the application that uses an embedded database and the Spring Framework JdbcTemplate, and then support building a web application.

Spring Boot will do all of that for us

-

```
// no configuration!
// Spring Boot will contribute all the things we contributed ourselves,
// manually, and so much more.

@SpringBootApplication
public class ApplicationConfiguration {

}
```

-
-

# Spring MVC-based controller to expose a REST endpoint to respond to HTTP GET

```
@RestController
public class CustomerRestController {
...
```

@RestController is another stereotype annotation, like @Component. It tells Spring that this component is to serve as a REST controller.

-
```
...
@GetMapping("/customers")
 public Collection<Customer> readAll() {
  return this.customerService.findAll();
 }
```

We can use Spring MVC annotations that map to the domain of what we’re trying to do: in this case, to handle HTTP GET requests to a certain endpoint with @GetMapping.

-

Then create some entry point

```
 public static void main(String [] args){
  SpringApplication.run (ApplicationConfiguration.class, args);
 }
```

-
-
# Cloud Foundry
Spring Boot lets us focus on the essence of the application itself, but what good would all that newfound productivity be if we then lost it all struggling to move the application to production?

If there’s a mistake in production, it should be as cheap as possible to fix it and deploy that fix.
-

Cloud Foundry is a cloud platform, a Platform as a Service `PaaS`

The focus is on applications and their services.





