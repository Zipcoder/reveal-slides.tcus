
# Object Design and Composition

-
-
# Object Ecosystems
* Our design process begins by answering the following:
	* What are my objects?
	* Where does `the object` come from?
	* Where does `the object` live?
	* How does `the object` interact with `other object`?

-
## Classes
### What are my objects?
* What are the properties (fields) of each object?
* What are the behaviors (methods) of each object?
* How many of each object do I need / expect?
	* Relational cardinality: 1-1, 1-M, M-1, M-M


-
## Object Creation
### Where does `the object` _come from_?
* Who / What entity creates `the object`?
* What class is responsible for `the object`'s construction?


-
## Object Scope
### Where does `the object` _go to_ (live)?
* What class _uses_ `the object`?
* What class is _dependent on_ `the object`?


-
## Object Mediation
### How does `the object`<br>interact with `other object`?
* What is the combined responsibility of this interaction?
* What is the _accessibility_ of `the object`?
* What is the _scope_ of `the object`?
* What class is responsible for causing this interaction?
	* The mediator


-
-
# Object Intentions
* Encapsulation
* Utility
* Creation
* Storing
* Managing / Handling / Decorating
* Contextualizing / Mediating / Scoping


-
-
# Encapsulation
* Wraps several data fields into a single entity
* In conversation, these _encapsulators_ are known as the "model".

```java
// class signature
public class Person {
	// instance variables (fields)
	private String name;
	private int age;
	private boolean isFemale;

	// constructor
	public Person(String name, int age, boolean isFemale) {
		this.name = name;
		this.age = age;
		this.isFemale = isFemale;
	}
}
```


-
-
# Utility Classes
* Defines a set of methods (typically static) that perform common functions (typically procedural)

```java
// static state; procedural behavior
public class RandomUtils {
    private static final Random random = new Random();

    public static Double createDouble(Double min, Double max) {
        return random.nextDouble() * (max - min) + min;
    }

    public static Integer createInteger(Integer min, Integer max) {
        return createDouble(min, max).intValue();
    }

    public static Character createCharacter(char min, char max) {
        return (char) createInteger((int) min, (int) max).intValue();
    }
}
```



-
-
# Creational Classes
* used to create other objects

```java
public class PersonFactory {
    public static Person createRandomPerson() {
        String name = RandomUtils.createString('a', 'z', 5);
        int age = RandomUtils.createInteger(0, 100);
        boolean isFemale = RandomUtils.createBoolean(50);

        Person randomPerson = new Person(name, age, isFemale);
        return randomPerson;
    }
}
```



-
# Container Classes
* store of data accumulated from within an ecosystem

```java
public class PersonWarehouse {
    private final Logger logger = Logger.getGlobal();
    private final List<Person> people = new ArrayList<>();

    public void addPerson(Person person) {
        logger.info("Registering a new person object to the person warehouse...");
        people.add(person);
    }

    public Person[] getPeople() {
    	return this.people.toArray(new Person[people.size()]);
    }
}
```

-
### Managers / Handlers / Decorators
* A class _wrapper_ which limits (facade) or extends (decorates) how a client can interact with an object.
```java
public class PersonHandler {
	private Person person;
	public PersonHandler(Person person) {
		this.person = person;
	}

	public void speak(String message) {
		String personName = this.person.getName();
		System.out.println(personName + " says, '" + message + "'");
	}

	public void sayAge() {
		int personAge = this.person.getAge();
		String spokenPhrase = "I am " + personAge + " years old.";
		speak(spokenPhrase);
	}
}
```


-
## Mediation (Scope)
* Where the objects live and interact collectively

```java
public class Main {
	public static void main(String[] args) {
		Person person1 = PersonFactory.createRandomPerson();
		Person person2 = PersonFactory.createRandomPerson();
		PersonWarehouse personWarehouse = new PersonWarehouse();
		personWarehouse.addPerson(person1);
		personWarehouse.addPerson(person2);

		Person[] people = personWarehouse.getPeople();
		for(Person currentPerson : people) {
			PersonHandler handler = new PersonHandler(currentPerson);
			handler.sayAge();
		}
	}
}
```
