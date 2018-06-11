# Object Oriented Programming
* Consider object ecosystem FIRST
	* What are my objects?
	* Where does `the object` come from?
	* Where does `the object` live?
	* How does `the object` interact with `other object`?

-
-
# Object Ecosystems
* What are my objects?
	* What are the properties (fields) of each object?
	* What are the behaviors (methods) of each object?
	* How many of each object do I need / expect?
* Where does `the object` come from?
	* Who / What creates `the object`?
* Where does `the object` live?
	* Where will `the object` be stored?
* How does `the object` interact with `other object`?

-
-
# Different Object Intentions
* Encapsulation
* Utilities
* Creation
* Storing
* Managing / Handling
* Entity Scope


-
-
# Encapsulation
* Wraps several data fields into a single entity

```
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
	private String myName; // instance variable	private boolean amFemale; // instance variable

	// no-arg constructor
	public Person() { // constructor signature
		this.myName = "Leon"; // setting instance variable
		this.amFemale = false; // setting instance variable
	}

	public Person(String name, boolean isFemale) { // constructor signature
		this.myName = name; // setting instance variable
		this.amFemale = isFemale; // setting instance variable
	}
}
```

-
## Calling Constructors<br>From Constructors
```java
public class Person { // class signature
	private String myName; // instance variable	private boolean amFemale; // instance variable

	// no-arg constructor
	public Person() { // constructor signature
		this("Leon", false); // nested constructor call
	}

	public Person(String name, boolean isFemale) { // constructor signature
		this.myName = name; // setting instance variable
		this.amFemale = isFemale; // setting instance variable
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
		this.myname = differentName; // setting instance variable
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
        boolean isMale = RandomUtils.createBoolean(50);

        Person randomPerson = new Person(name, age, isMale);
        return randomPerson;
    }
}
```



-
# Warehouse Classes
* store of data accumulated from within an ecosystem

```java
public class PersonWarehouse {
    private static final Logger logger = Logger.getGlobal();
    private static final ArrayList<Person> people = new ArrayList<>();

    public static void addPerson(Person person) {
        logger.info("Registering a new person object to the person warehouse...");
        people.add(person);
    }

    public static Person[] getPeople() {
    	return people.stream().toArray(Person[]::new);
    }
}
```

-
## Managers / Handlers
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
## Scope
* Where the objects live and interact collectively

```java
public class Main {
    public static void main(String[] args) {
        PersonFactory.createRandomPerson();
        PersonFactory.createRandomPerson();
        PersonFactory.createRandomPerson();
        PersonFactory.createRandomPerson();
        PersonFactory.createRandomPerson();

        Person[] people = PersonWarehouse.getPeople();

        int currentIndex = 0;
        while(currentIndex < people.length) {
            Person currentPerson = people[currentIndex];
            PersonHandler personHandler = new PersonHandler(currentPerson);
            System.out.println(personHandler.sayAge());
            currentIndex++;
        }
    }
}
```
