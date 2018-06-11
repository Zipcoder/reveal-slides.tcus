# Spring Dependency Injection



-
-
### DI in Spring

The Spring Framework IoC (Inversion of Control) container handles Dependency Injection by registering (learning about) Spring beans and providing them to satisfy dependencies as needed.

-
### @Configuration

- Used for Java-based configuration
- Creates a configuration Spring Bean
- `@Bean`-annotated methods define other beans to be registered
- Pull in other configuration beans with `@Import`

-
### Spring Beans

- Managed by Spring
- Created as singletons
  - Use `@Scope` to change this
  - Subsequent requests produce the same instance

-
### @ComponentScan

- Finds and registers components in subdirectories of annotated class
  - `@Component`, `@Controller`, `@Service`, `@Repository`
- default in `@SpringBootApplication`

-
from [Spring Boot Reference Guide]():

```Java
package com.example.myproject;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// same as @Configuration @EnableAutoConfiguration @ComponentScan
@SpringBootApplication 
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

-
### @Autowired

- Autowired dependencies will be filled by registered beans
- Only unambiguous dependencies will be fulfilled
- Use `@Qualifier` or `@Profile` to narrow down choices


-
-
### Autowiring in tests

```Java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=MyApplicationConfig.class)
public class MyApplicationTest {

	@Autowired
	TestingDependency td;
	
	@Test
	public void testMethod(){...}
```

