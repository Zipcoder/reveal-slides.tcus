#  Twelve-Factor Application Style Configuration

-
-

# The Confusing Conflation of “Configuration”

When we talk about `configuration` in Spring

we’ve usually talked about the inputs into the Spring framework’s various ApplicationContext implementations that help the container understand how you want beans wired together.

-

This might be an XML file to be fed into a ClassPathXmlApplicationContext, or Java classes annotated a certain way to be fed into an AnnotationConfigApplicationContext. Indeed, when we talk about the latter, we refer to it as `Java configuration`.

however, we’re going to look at configuration as it is defined in the Twelve-Factor application manifesto

-

In this instance, configuration refers to literal values that may change from one environment to another: 
things like 
1. passwords
2. ports 
3. hostnames
4. feature flags.

-
-

# Support in Spring Framework
Spring has supported twelve-factor–style configuration since the PropertyPlaceholderConfigurer class was introduced.

Once an instance is defined, it replaces literals in the XML configuration with values that it resolved in a .properties file.

Twelve-factor–style configuration aims to eliminate the fragility of having magic strings—values like database locators and credentials, ports, etc.—hardcoded in the compiled application.

-
-

# The PropertyPlaceholderConfigurer
A Spring XML configuration file

```
<context:property-placeholder location="classpath:some.properties"/>  <---

<bean class="classic.Application">
    <property name="configurationProjectName" value="${configuration.projectName}"/>
</bean>
```

A classpath: location refers to a file in the current compiled code unit (.jar, .war, etc.). Spring supports many alternatives, including file: and url:, that would let the file live external to the compiled unit.

_
-

# The Environment Abstraction and @Value

The Environment abstraction provides a bit of runtime indirection between the running application and the environment in which it is running, and lets the application ask questions

-

You can tell Spring to load up configuration keys from a file, specifically and in a fashion similar to what you may have done with earlier editions of Spring’s property placeholder resolution, using the @PropertySource annotation.

-

The @Value annotation provides a way to inject environment values into constructors, setters, fields, etc. These values can be computed using the Spring Expression Language or using the property placeholder syntax, assuming one registers a PropertySourcesPlaceholderConfigurer

-

```
@Configuration
@PropertySource("some.properties")
public class Application {
...
```

The @PropertySource annotation is a shortcut, like property-placeholder, that configures a PropertySource from a .properties file.

-

```
 @Bean
 static PropertySourcesPlaceholderConfigurer pspc() {
  return new PropertySourcesPlaceholderConfigurer();
 }
```

You need to register the PropertySourcesPlaceholderConfigurer as a static bean, because it is an implementation of BeanFactoryPostProcessor and must be invoked earlier in the Spring bean initialization life cycle. This nuance is invisible when you’re using Spring’s XML bean configuration.

-

```
 @Value("${configuration.projectName}")
 private String fieldValue;
```

You can decorate fields with the @Value annotation **but don’t! it frustrates testing**

-

```
@Autowired
 Application(@Value("${configuration.projectName}") String pn) {
  log.info("Application constructor: " + pn);
 }
```
or you can decorate constructor parameters with the @Value annotation

-

```
@Value("${configuration.projectName}")
 void setProjectName(String projectName) {
  log.info("setProjectName: " + projectName);
 }
```

or you can use setters methods

-

```
 @Autowired
 void setEnvironment(Environment env) {
  log.info("setEnvironment: " + env.getProperty("configuration.projectName"));
 }
```

or you can inject the Spring Environment object and resolve the key manually.

-

```
@Bean
 InitializingBean both(Environment env,
  @Value("${configuration.projectName}") String projectName) {
  return () -> {
   log.info("@Bean with both dependencies (projectName): " + projectName);
   log.info("@Bean with both dependencies (env): "
    + env.getProperty("configuration.projectName"));
  };
 }

```

You can use @Value annotatedparameters in Spring Java configuration @Bean provider method arguments, as well.

-

This example loads up the values from a file, simple.properties, and then has one value, configuration.projectName, made available in a variety of ways.

-
-

# Profiles

The Environment also brings in profiles. It lets you ascribe labels (profiles) to groupings of beans. Use profiles to describe beans and bean graphs that change from one environment to another.

-

You can activate one or more profiles at a time. Beans that do not have a profile assigned to them are always activated. 

Beans that have the profile default are activated only when there are no other active profiles.

-

Profiles let you describe sets of beans that need to be created differently in one environment versus another. 

You might, for example, use an embedded H2 javax.sql.DataSource in your local dev profile, but then switch to a javax.sql.DataSource for PostgreSQL

-

```
 @Configuration
 @Profile("prod")
 @PropertySource("some-prod.properties")
 public static class ProdConfiguration {

  @Bean
  InitializingBean init() {
   return () -> LogFactory.getLog(getClass()).info("prod InitializingBean");
  }
 }

 @Configuration
 @Profile({ "default", "dev" })
```
-

This configuration class and all the @Bean definitions therein will only be evaluated if the prod profile is active.

-

```
 @PropertySource("some.properties")
 public static class DefaultConfiguration {

  @Bean
  InitializingBean init() {
   return () -> LogFactory.getLog(getClass()).info("default InitializingBean");
  }
 }
```
This configuration class and all the @Bean definitions therein will only be evaluated if the dev profile or no profile—including dev—is active.
-
```
 @Bean
 InitializingBean which(Environment e,
                        @Value("${configuration.projectName}") String projectName) {
  return () -> {
   log.info("activeProfiles: '"
           + StringUtils.arrayToCommaDelimitedString(e.getActiveProfiles()) + "'");
   log.info("configuration.projectName: " + projectName);
  };
 }

```
This InitializingBean simply records the currently active profile and injects the value that was ultimately contributed by the property file.

-

```
public static void main(String[] args) {
  AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext();
  ac.getEnvironment().setActiveProfiles("dev"); 4
  ac.register(Application.class);
  ac.refresh();
 }
```
It’s easy to programmatically activate a profile (or profiles).

-
-

# Bootiful Configuration

Spring Boot improves things considerably. Spring Boot will automatically load properties from a hierarchy of well-known places by default. 

The command-line arguments override property values contributed from JNDI, which override properties contributed from System.getProperties(), etc.

-

- Command-line arguments

- JNDI attributes from java:comp/env

- System.getProperties() properties

- OS environment variables

- External property files on filesystem: (config/)?application.(yml.properties)

- Internal property files in archive (config/)?application.(yml.properties)

- @PropertySource annotation on configuration classes

- Default properties from SpringApplication.getDefaultProperties()

-

## An application.yml property file. Data is hierarchical
```
configuration:
  projectName : Spring Boot
management:
  security:
    enabled: false
```

-
Properties are resolved automatically from src/main/resources/application.yml

-

```
@EnableConfigurationProperties
@SpringBootApplication
public class Application {

 private final Log log = LogFactory.getLog(getClass());

 public static void main(String[] args) {
  SpringApplication.run(Application.class);
 }
```

The @EnableConfigurationProperties annotation tells Spring to map properties to POJOs annotated with @ConfigurationProperties.

-

```
@Component
@ConfigurationProperties("configuration")
class ConfigurationProjectProperties {

 private String projectName; 3

 public String getProjectName() {
  return projectName;
 }
```

@ConfigurationProperties tells Spring that this bean is to be used as the root for all properties starting with configuration., with subsequent tokens mapped to properties on the object.

-

```
private String projectName; 3

 public String getProjectName() {
  return projectName;
 }
```

The projectName field would ultimately have the value assigned to the property key configuration.projectName.

-
-

# Centralized, Journaled Configuration with the Spring Cloud Configuration Server

We’re still missing answers for some common use cases

-

- Changes to an application’s configuration, as configured, would require restarts.

- There is no traceability: how do we determine what changes were introduced into production and, if necessary, roll back those changes?

- Configuration is decentralized; it’s not immediately apparent where to go to change what.

- There is no out-of-the-box support for encryption and decryption for security.

-

# The Spring Cloud Config Server
The Spring Cloud Config Server is a REST API to which our clients will connect to draw their configuration.

It manages a version-controlled repository of configuration

The Spring Cloud Config client contributes a new scope to client applications, the refresh scope, that allows us to reconfigure Spring components without restarting the application.

-

# Spring Cloud Config Clients
The Config Server manages a directory full of .properties or .yml files. Spring Cloud serves configuration to connected clients by matching the client’s spring.application.name to the configuration file in the directory. 

Thus, a configuration client that identifies itself as foo-service would see the configuration in foo-service.yml or foo-service.properties.

-

Run the Config Server and verify that your configuration service is working; point your browser to http://localhost:8888/SERVICE/master where SERVICE is the name you specify as spring.application.name

-

The Spring Cloud Config Server returns client-specific configuration by matching on the spring.application.name, but it can return global configuration—configuration that is visible to every client—from a file named application.properties or application.yml in the Spring Cloud Config Server’s repository.

-

The configuration service returns JSON that contains all the configuration values in the application.(properties,yml) file as well as any service-specific configuration in configuration-client.(yml,properties).

It will also load any configuration for a given service and a specific profile, e.g., configuration-client-dev.properties, where configuration-client is the name of the client and dev is the name of a profile under which the client is running.

-

The values in the Spring Cloud Config Server are, from the perspective of any Spring Boot application, just another PropertySource, and resolvable in all the usual ways: through @Value-annotated class members as well as from the Environment abstraction.

-

# Security

-

You can protect the Spring Cloud Configuration Server itself with HTTP BASIC authentication as well. The easiest is to just include org.springframework.boot : spring-boot-starter-security and then define a security.user.name and a security.user.password property.

-

The spring-boot-starter-security starter brings in Spring Security, so you could plug in a particular Spring Security UserDetailsService implementation to override how authentication is handled.

-

The Spring Cloud Config Clients can encode the user and password in the spring.cloud.config.uri value—for example, https://user:secret@host.com.

-
-
# Refreshable Configuration
Centralized configuration is a powerful thing, but changes to configuration aren’t immediately visible to the beans that depend on it.

Spring Cloud’s refresh scope (and the convenient @RefreshScope annotation) offer a solution. 

-


```
@RestController
@RefreshScope
class ProjectNameRestController {

 private final String projectName;
```
The @RefreshScope makes this bean refreshable.

-

```
 @Autowired
 public ProjectNameRestController(
  @Value("${configuration.projectName}") String pn) { 2
  this.projectName = pn;
 }
```

Spring resolves the configuration value from the Config Server, which is just another PropertySource in the Environment.

-

The ProjectNameRestController is annotated with the @RefreshScope annotation, a Spring Cloud-contributed scope that lets any bean recreate itself in place.

@Value and @Autowired injects reestablished—whenever a refresh event is triggered.

Fundamentally, all refresh-scoped beans will refresh themselves when they receive a Spring ApplicationContext event of the type RefreshScopeRefreshedEvent.

-

```
 private final AtomicLong counter = new AtomicLong(0); 1

 2
 @EventListener
 public void refresh(RefreshScopeRefreshedEvent e) {
  this.log.info("The refresh count is now at: "
   + this.counter.incrementAndGet());
 }
 
```
1 The component keeps an atomic counter...
2 ..that updates itself every time a RefreshScopeRefreshedEvent is observed

-

# Summary

it should be easy to package one artifact and then move that artifact from one environment to another without changes to the artifact itself.





