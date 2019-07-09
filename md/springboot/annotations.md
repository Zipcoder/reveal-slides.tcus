# Basic Annotations

-
-

## Annotations and Configuration

Spring looks for classes on the class path configured for different jobs. We can use annotations to configure classes expressively for Spring.

-

## SpringBootApplication

The `@SpringBootApplication` annotation is actually a few different annotations wrapped up into one. 

* @EnableAutoConfiguration
* @ComponentScan
* @Configuration

-

## Entity

The `@Entity` annotation is a control from JPA that allows you to define a class that will be mapped to a relational database table

```Java
@Entity //Lets JPA know to create a table based on this class
public class User {
	...
}
```

-

## Id

All Entities need at least one field to act as a unique identifier. Typically an identifier in Java will be a Long, though you can change this if you'd like. 

```Java
@Id //Makes this attribute the unique identifier in the database
private Long uniqueIdentifier;
```
-

## Component

The `@Component` annotation is as generic as an annotation gets. It means that the class will be found by @ComponentScan when the app is loaded but applies no additional context to the class

A class found with ComponentScan can be used later by SpringBoot

-

## Repository

A `@Repository` is a type of Component that will be discovered with `@ComponentScan`

a repository is a mechanism for encapsulating storage, retrieval, and search behavior.

```Java
@Repository //Responsible for interacting with the database to 
			//collect and store User objects
public interface BakerRepository extends CrudRepository<User, Long> {
}
```

-

## Service

The `@Service` annotation is a specialized type of component. It indicates a class designed for a task and can be later injected into our controllers. Notably, `@Service` and `@Component` are in fact functionally identicle.

Noted in documentation: "It’s a good idea to use @Service over @Component in service-layer classes because it specifies intent better. Additionally, tool support and additional behavior might rely on it in the future."

-

## Service Example

```Java
@Service // Marks the class as a component, specifically a service
public class MathService {
	// Methods like these will be useful later when we write controllers
	public int add(int x, int y) { return x+y; }
}
```

-

## Controller

A class annotated with `@Controller` marks it as a special component designed to be a controller. Controllers will be responsible for passing data as a model to a view or to take data from the backend and pass it to a backing service for storage.

Controllers are often used in tandem with request handlers

-

## RequestMapping

The `@RequestMapping` annotation marks a function in a controller. Depending on the values passed in, it will handle different HTTP messages

```Java
// when an HTTP message is sent to the /users route with the GET verb
// spring will use this method to get the response.
@RequestMapping(value = "/users", method = RequestMethod.GET)
public ResponseEntity<Iterable<User>> index() {
	// We return response entities which are able to be converted
	// to a view in the form of either json or xml
    return new ResponseEntity<>(userService.findAll());
}
```

-

## RequestMapping cont.
RequestMapping defaults to RequestMethod.GET for its verb, but can take any verb supported by HTTP. The main ones are GET, PUT, POST, and DELETE.

There are also shortcuts for these

* `@GetMapping`
* `@PostMapping`
* `@PutMapping`
* `@DeleteMapping`

-

## PathVarible

You can further generify your RequestMapping by having varibles in your path. You may mark a method argument with `@PathVaribale` to tell Spring to fill that argument with the path varible. 

```Java
// We wrap path pieces with `{}` to mark them as varibles.
// This path will match users/1 or users/328 or any Long value. 
@GetMapping("/users/{id}")
public ResponseEntity<User> show(@PathVariable Long id) {
	// The argument was marked with @PathVaribale so the id
	// within the method will be whatever was put into the
	// route by the client. e.g. users/1 will result in id
	// being equal to 1
    return new ResponseEntity<>(userService.findOne(id));
}

```
-

## RequestBody
Sometimes a client will send an HTTP message with a body of data for Spring to consume. This can be passed into a method argument using the `@RequestBody` annotation. As long as the object used has a constructor that is suitable to create the object, the request body will be passed in as a Java object. 

```Java
@PostMapping("/users")
public ResponseEntity<User> create(@RequestBody User user) {
	// When a client sends us a user to store, we first have to map
	// it into a user object. Because we use the @RequestBody 
	// annotation Spring will kindly do this for us. From here we 
	// can simply persist the user with our userService
    return new ResponseEntity<>(userService.create(user));
}
```

-
## Autowired

The Autowired annotation asks Spring to wire a dependency into a class from the Application Context. Spring will only be able to Autowire a dependency if it has a Bean capable of satisfying that dependency in the Application Context.

By default, any `@Component` will be in the Application Context

-
## Autowired

```Java
// Here we are creating a UserController. That controller requires
// the UserService, so we will tell Spring to autowire it in
@Controller
public class UserController {

	@Autowired
	private UserService userService;

	@Autowired
	private MathService mathService;

	@GetMapping("/users/{id}")
	public ResponseEntity<User> show(@PathVariable Long id) {
		// Since the dependency was annotated with @Autowired
		// it will be instantiated by Spring before we get to
		// this line ensuring we can use it
	    return new ResponseEntity<>(userService.findOne(id));
	}

}
```
-

## Autowired

```Java
@Controller
public class UserController {

	private UserService userService;
	private MathService mathService;

	// We can also wire in multiple dependencies 
	// through the constructor
	@Autowired
	public UserController(UserService userService, MathService mathService)
	{
		this.userService = userService;
		this.mathService = mathService;
	}
	// Both methods will work using Spring's testing, though you may
	// need to use constructor based autowiring if you are using a
	// different testing framework from Spring's default one

}
```
