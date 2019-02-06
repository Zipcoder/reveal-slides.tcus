# Object Design and Composition

-
-
# Object Design and Composition
* Consider object ecosystem FIRST
	* What are my objects?
	* Where does `the object` come from?
	* Where does `the object` live?
	* How does `the object` interact with `other object`?

-
-
# Object Ecosystems
* The design process of a new object oriented program begins by answering the following:
	* What are my objects?
		* What are the properties (fields) of each object?
		* What are the behaviors (methods) of each object?
		* How many of each object do I need / expect?
			* Relational cardinality: 1-1, 1-M
	* Where does `the object` come from?
		* Who / What entity creates `the object`?
	* Where does `the object` live?
		* What is the scope of `the object`?
	* How does `the object` interact with `other object`?
		* object mediation

-
-
# Different Object Intentions
* Encapsulation
* Utility
* Creation
* Storing
* Managing / Handling / Decorating
* Mediation (scoping)


-
-
# Encapsulation
* Wraps several data fields into a single entity

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
# Class Signature
```java
public class Person { // class signature
}
```

-
# Instance Variables<br>(Fields)
```java
public class Person { // class signature
	private String name; // instance variable
}
```

-
# Constructor<br>(Default)
```java
public class Person { // class signature
	private String name; // instance variable

	public Person() { // constructor signature
	}
}
```

-
# Constructor<br>(Non-Default)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}
}
```

-
# Multiple Constructors
```java
public class Person { // class signature
	private String myName; // instance variable

	// no-arg (default) constructor
	public Person() { // constructor signature
		this.myName = "Leon"; // setting instance variable
	}

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}
}
```

-
## Calling Constructors<br>From Constructors
```java
public class Person { // class signature
	private String myName; // instance variable
	private Character gender; // instance variable

	// no-arg constructor
	public Person() { // constructor signature
		this.myName = "Leon"; // setting instance variable
		this.myGender = 'M'; // setting instance variable
	}

	public Person(String name, Character gender) { // constructor signature
		this.myName = name; // setting instance variable
		this.myGender = gender; // setting instance variable
	}
}
```

-
## Calling Constructors<br>From Constructors
```java
public class Person { // class signature
	private String myName; // instance variable
	private Character myGender; // instance variable

	// no-arg constructor
	public Person() { // constructor signature
		this("Leon", 'M'); // nested constructor call
	}

	public Person(String name, Character gender) { // constructor signature
		this.myName = name; // setting instance variable
		this.myGender = gender; // setting instance variable
	}
}
```

-
# Setters<br>(Mutators)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}

	public void setName(String differentName) { // method signature
		this.myName = differentName; // setting instance variable
	}
}
```

-
# Setters<br>(Mutators)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		setName(name); // method can be overriden; bad design
	}

	public void setName(String differentName) { // method signature
		this.myname = differentName; // setting instance variable
	}
}
```

-
# Getters<br>(Accessors)
```java
public class Person { // class signature
	private String myName; // instance variable

	public Person(String name) { // constructor signature
		this.myName = name; // setting instance variable
	}

	public void setName(String differentName) { // method signature
		this.myname = differentName; // setting instance variable
	}

	public String getName() {
		return this.myName;
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

    public static Float createFloat(float min, float max) {
        return random.nextFloat() * (max - min) + min;
    }

    public static Integer createInteger(Integer min, Integer max) {
        return createFloat(min, max).intValue();
    }

    public static Character createCharacter(char min, char max) {
        return (char) createInteger((int) min, (int) max).intValue();
    }

    public static String createString(char min, char max, int stringLength) {
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < stringLength; i++) {
            sb.append(createCharacter(min, max));
        }
        return sb.toString();
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
## Managers / Handlers / Decorators
```java
public class PersonHandler {
	private Person person;
	public PersonHandler(Person person) {
		this.person = person;
	}

	public void speak(String message) {
		String personName = this.person.getName();
		String outputMessage = personName + " says, '" + message + "'";
		System.out.println(outputMessage);
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
		Person person3 = PersonFactory.createRandomPerson();
		PersonWarehouse personWarehouse = new PersonWarehouse();
		personWarehouse.addPerson(person1);
		personWarehouse.addPerson(person2);
		personWarehouse.addPerson(person3);

		Person[] people = personWarehouse.getPeople();

		int currentIndex = 0;
		while(currentIndex < people.length) {
			Person currentPerson = people[currentIndex];
			PersonHandler personHandler = new PersonHandler(currentPerson);
			personHandler.sayAge();
			currentIndex++;
		}
	}
}
```
