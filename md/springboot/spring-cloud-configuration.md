##  Twelve-Factor Application Style Configuration

-
-

## Twelve-Factor Style Configuration

Up till now, we've used the term ``configuration`` when discussing how are beans are registered and used by other beans.
Today, we are going to use the term  configuration to refers to literal values that may change from one environment to another, including:

* Spring Properties
* Application Info and Sensitive Data
    * application info
    * passwords
    * ports 
    * hostnames
-
-

## What is an Environment?

An environment, concretely, is the server where our application is deployed and executed.

Abstractly, environment describes a stage in our application's lifecycle, from *development*, to *testing* and through to *production*, and the settings we apply to the server for that stage in the lifecycle.

-

## Overriding Spring Properties with Application.properties

Spring makes numerous assumptions about how you want your application to run through configurable properties. You can overwrite various spring defaults through the ``application.properties``
file, located in ``src/main/resources/application.properties``.

```
# Base path to be used by Spring Data REST to expose repository resources.
spring.data.rest.base-path=api
# Enable debug logs.
debug=false
```

Spring stores the values for properties as a simple string that you can access via its key.

<a href="https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html">Common configurable properties</a>

-

## Overriding Spring Properties with Application.properties

The properties available for us to override are exposed based on the classes available in the classpath.

If we don't include a property in ``application.properties``, Spring will plug in defaults.

-

## Environment Properties

Environment Properties are variables set on the server that provide context for the application running in that environment. Some examples might be: 

* PORT – The port on which the app should listen for requests.
* HOME - Root folder for the deployed application.                                                                       
* PWD – the current working directory.
* HOSTNAME – the hostname of the computer.
* USER - The user account under which the application runs.
* DATABASE_URL - The location of our backing database

-

## Environment Properties

Environments contain standard properties that describe information about the server. 

But we can also add, name and set additional environment properties specific to our application.

Environment properties are the best place to store sensitive information like passwords.

-

## Eliminating Hardcoded Values with Environment Properties

Our applications rely on concrete values for database locations, database credentials, usernames, passwords and more.

When those values are hardcoded, it makes it impossible to maintain one codebase that can be deployed across different environments.

Instead, we can reference environment variables within our application, so that those values will be plugged in when we deploy.

-

## Using Property Values in Spring

The ``@Value`` annotation provides a way to assign a property value to field.

Given an ``application.properties`` file:

```
value.from.property=Value from the property
project.name=Project Name
priority=Application property
```
In the following example we get “Value from the property” assigned to the field:
```
 @Value("${value.from.property}")
 private String valueFromProperty;
```

-

## Using Property Values in Spring

We can also set the value from environment properties with the same syntax. Let’s assume that we have defined a environment property named ENVIRONMENT_PROPERTY.

```
@Value("${environment.property}")
private String environmentProperty;
```

We can replace underscores or dashes in environment variables with a dot to access the value.

-

## Priority When Using Property Values in Spring

Suppose we had a property priority defined as a environment property with the value “Environment property” and defined as something else in the properties file. 

```
@Value("${priority}")
private String priorityEnvironmentProperty;
```

In the above code the value would be “Environment property”.

-

## Using Property Values in Spring

Default values can be provided for properties that might not be defined. In this example the value “some default” will be injected when ``unknown.param`` is not found.

```
@Value("${unknown.param:some default}")
private String someDefault;
```

-

## Using Property Values in Setters

You can also use ```@Value``` to inject setter method parameters.

```
@Value("${configuration.projectName}") 
void setProjectName(String projectName) {  
    this.projectName = projectName;
}
```
-

## Using Property Values in Constructors

You can also use ```@Value``` to inject constructor parameters.

```
@Autowired
Application(@Value("${project.name}") String pn) {
    log.info("Application constructor: " + pn);
}
```

-

## Mapping Property Values to POJO fields

Injecting individual fields or parameters with values is fun, but we can do better.

The ``@EnableConfigurationProperties`` annotation tells Spring to map properties to POJOs annotated with ``@ConfigurationProperties``.

```
@EnableConfigurationProperties
@SpringBootApplication
public class Application {

 public static void main(String[] args) {
  SpringApplication.run(Application.class);
 }
```

-

## Mapping Property Values to POJO fields

``@ConfigurationProperties`` tells Spring that this bean is to be used as the root for all properties starting with a specific name, with subsequent fields mapped to properties on the object.

Let's look at this ``application.propeties`` file for the following example.

```
configuration.projectName=Project Name
configuration.projectOwner=Project Owner
configuration.projectDescription=Project Description
```
-

## Mapping Property Values to POJO fields

We can easily add ``@ConfigurationProperties`` to map numerous properties from a property group to individual fields.
In this case, we want to map all the properties that begin with "configuration" to the ConfigurationProjectProperties class. 

```
@Component
@ConfigurationProperties("configuration")
class ConfigurationProjectProperties {

 private String projectName;
 
 private String projectOwner; 
 
 private String projectDescription; 

```

-

## Mapping Property Values to POJO fields

```
@Component
@ConfigurationProperties("configuration")
class ConfigurationProjectProperties {

 private String projectName;
 
 private String projectOwner; 
 
 private String projectDescription; 

```

The projectName, projectOwner, and projectDescription fields would automatically have the value assigned to the property attached to ``configuration.projectName``, ``configuration.projectOwner``, and ``configuration.projectDescription``.

-
-

### Centralized Configuration with the Spring Cloud Configuration Server

We’re still missing answers for some common use cases

* Changes to an application’s configuration, as configured, would require restarts.
* There is no traceability: how do we determine what changes were introduced into production and, if necessary, roll back those changes?
* Configuration is decentralized; it’s not immediately apparent where to go to change what.
* There is no out-of-the-box support for encryption and decryption for security.

-

## The Spring Cloud Config Server

The Spring Cloud Config Server is a REST API to which our clients will connect to draw their configuration.

The Config Server manages a version-controlled repository full of ``.properties`` files.

The Spring Cloud Config client contributes a new scope to client applications, the refresh scope, that allows us to reconfigure Spring components without restarting the application.

-

## Creating A Spring Cloud Config Server

Add the following dependency to your pom.xml

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

Then, annotate your main Application class with ``@EnableConfigServer``.

```
@SpringBootApplication
@EnableConfigServer
public class ConfigServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServiceApplication.class, args);
	}
}
```
-

## Configuring Spring Cloud Config Server 

In our ``application.properties`` we need to provide some key info for the config server.

```
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/dominiqueclarke/spring-config-server
```
Set your ``server.port`` to a port separate from the services that will rely on the config server.

Set your ``spring.cloud.config.server.git.uri`` to the uri of a git repository (on github, bitbucket, or elsewhere) that contains the config repository.

-

## Spring Cloud Config Clients

Add the following dependency to your pom.xml

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```
-

## Spring Cloud Config Clients

In order to configure a service to use spring cloud client, we need a new type of file, a ``bootstrap.properties`` file. 

Bootstrap.properties is loaded before ``application.properties``, and provides information about how to "bootstrap" an applications properties by grabbing them from a config server.

-

## Spring Cloud Config Clients

In order to point our client to the config server, we need to add the following to ``bootstrap.properties``

```
spring.application.name=sample-service
spring.cloud.config.uri=http://localhost:8888
```
Spring Cloud serves configuration to connected clients by matching the client’s ``spring.application.name`` to the configuration file in the repository. Thus, a configuration client that identifies itself as ``foo-service`` would see the configuration or ``foo-service.properties``.

The ``spring.cloud.config.uri`` property contains the uri of where the config server is hosted.

-

## Global Vs Client-Specific Configuration

The Spring Cloud Config Server returns client-specific configuration by matching on the ``spring.application.name``.

However, it can return global configuration that is visible to every client from a file named application.properties in the Spring Cloud Config Server’s repository.

The configuration service returns JSON that contains all the configuration values in the ``application.properties`` file as well as any service-specific configuration in ``.properties`` file named after the service.

-

## Refreshable Configuration
Centralized configuration is a powerful thing, but changes to configuration aren’t immediately visible to the beans that depend on it.

Spring Cloud’s refresh scope (and the convenient @RefreshScope annotation) offer a solution. 

```
@RestController
@RefreshScope
class ProjectNameRestController {

 private final String projectName;
```
The @RefreshScope makes this bean refreshable.

-

## @Refresh Scope

The ProjectNameRestController is annotated with the @RefreshScope annotation, a Spring Cloud-contributed scope that lets any bean recreate itself in place.

@Value and @Autowired injects reestablished—whenever a refresh event is triggered.

Fundamentally, all refresh-scoped beans will refresh themselves when they receive a Spring ApplicationContext event of the type RefreshScopeRefreshedEvent.

