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
from [Spring Boot Reference Guide](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-using-springbootapplication-annotation):

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
### @Autowired:  Disambiguation - @Qualifier
Spring resolves @Autowired entries by type. If multiple beans of the same type are available, a fatal exception will be thrown.
(ie, if the bean share a common SuperClass or Interface)

```Java
@Component("tigerTony")
public class Tony implements Tiger {
    public String speak() {
        return "Grrrrreat!";
    }
}
```
```Java
@Component("tigerTigger")
public class Tigger implements Tiger {
    public String speak() {
        return "hoo-hoo-hoo-hoo-oo-oo-oo!";
    }
}
```
-
### @Autowired:  Disambiguation - @Qualifier

```Java
public class TiggerService {
    @Autowired
    private Tiger tiger;
}
```

will result in the following error: 

```Java
Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: 
No qualifying bean of type [com.autowire.sample.Tiger] is defined: 
expected single matching bean but found 2: tigerTigger,tigerTony
```

-
### @Autowired:  Disambiguation - @Qualifier

User the @Qualifier annotation to avoid that error:

```Java
public class TiggerService {
    @Autowired
    @Qualifier("tigerTigger")
    private Tiger tiger;
 
}
```

-
### @Autowired:  Disambiguation - @Profile
Using @Profile annotation, you can applay conditional usage on beans or components of the same name:

```Java
@Component("dbJawn")
@Profile("prod")
public class ProdDB implements DBJawn {
    @Override
    public boolean isLive() {
        return true;
    }
}
```

```Java
@Component("dbJawn")
@Profile("dev")
public class DevDB implements DBJawn {
    @Override
    public boolean isLive() {
        return false;
    }
}
```
-
### @Autowired:  Disambiguation - @Profile
Refine the @Autowired behavior by applying the desired @Profile to the component that uses it:

```Java
@Component
@Profile("dev")
public class DBJawnUser {
    @Autowired
    DBJawn dbJawn;
    ...
}
```

-
-
<img src="img/bunnies/11-Bunny-Picture.jpg" width="80%" style="margin:0 10%" >
