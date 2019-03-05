# Design patterns

## Creational Patterns



-
-
### Singleton Pattern
* Problem:
	* Ensure a class only has one instance and provide a global access point to it.
* Different Archetypes:
	* Eager initialization
	* Lazy initialization
	* Enum singleton
 	* Bill Pugh singleton

-
### Requiring a global access Point
* Consider a Casino application wherein a `Player` instance require reference to an _externally persistent_ `Profile`.

```java
public class GoFishGame {
  private List<GoFishPlayer> playerList;

  public void createPlayer() {
    String userPrompt = "What is your profile ID?";
    IOConsole console = new IOConsole();
    ProfileManager profileManager = ProfileManager.getInstance();

    Integer profileId = console.getIntegerInput(userPrompt);
    Profile profile = profileManager.getById(profileId);
    GoFishPlayer player = new GoFishPlayer(profile);
    playerList.add(player);
  }
}
```

* Here, the `ProfileManager` is a singleton.


-
### Eager Initialization
* **Solution:** the instance of Singleton Class is created at the time of class loading
* **Consequence:** instance is created even if client application might not be using it.

```java
public class BankAccountManager {    
    private static final BankAccountManager instance = new BankAccountManager();

    private BankAccountManager(){}

    public static BankAccountManager getInstance(){
        return instance;
    }
}
```

-
### Lazy Initialization
* **Solution:** creates the instance in the global access method.
* **Consequence:** a bit verbose

```java
public class LazySingleton {
    private static LazySingleton instance;

    private LazySingleton(){}

    public static LazySingleton getInstance(){
        if(instance == null){
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

-
### Bill Pugh Singleton
* **Solution:**
	* inner class is only loaded upon invokation of `getInstance`.
	* doesn't require synchronization
* **Consequence:**

```java
public class BillPughSingleton {

    private BillPughSingleton(){}

    private static class SingletonHelper{
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance(){
        return SingletonHelper.INSTANCE;
    }
}
```

-
### Enum Singleton
* **Solution:** Java ensures that any enum value is instantiated only once.
* **Consequence:** does not allow lazy initialization.

```java
public enum EnumSingleton {
    INSTANCE;
}
```

-
### Usage
* Notice the two different syntaxes for referencing a singleton object.

```java
public class Main {
	public static void main(String[] args) {
		PersonFactory personFactory = PersonFactory.getInstance();
		PersonWarehouse personWarehouse = PersonWarehouse.INSTANCE;

		Person person1 = personFactory.createPerson();
		Person person2 = personFactory.createPerson();
		Person person3 = personFactory.createPerson();

		personWarehouse.add(person1, person2, person3);
	}
}
```


-
-
## Creational Patterns: Factory
* Defers some part of object construction to another class
* [Factory Method](_)
	* A method which is responsible for instantiating and returning an object.
* [Factory Pattern](_)
	* A system which includes a class solely responsible for creation of another.
* Abstract Factory Class Pattern
	* Provides an interface for creating families of related or dependent objects without specifying their concrete classes.



-

There are three major issues with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.
1. Too Many arguments to pass from client program to the Factory class that can be error prone because most of the time, the type of arguments are same and from client side its hard to maintain the order of the argument.
2. Some of the parameters might be optional but in Factory pattern, we are forced to send all the parameters and optional parameters need to send as NULL.
3. If the object is heavy and its creation is complex, then all that complexity will be part of Factory classes that is confusing.



-
-
## Creational Patterns: Builder
* Separate the construction of a complex object from its representation so that the same construction process can create different representations

* Builder pattern was introduced to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.


-
-
### Builder Pattern


-
### Brief Example, Part 1
* Consider an application wherein a `License` can consist of many different fields, often having null values.

```java
public class License {
    String name, addressLine1, addressLine2, city, state;
    Date birthDate, issuedDate, expirationDate;
    Integer licenseNumber, zipcode;

    public License(String name, String addressLine1, String addressLine2,
                   String city, String state, Date birthDate,
                   Date issuedDate, Date expirationDate,
                   Integer licenseNumber, Integer zipcode) {
        this.name = name;
        this.addressLine1 = addressLine1;
        this.addressLine2 = addressLine2;
        this.city = city;
        this.state = state;
        this.birthDate = birthDate;
        this.issuedDate = issuedDate;
        this.expirationDate = expirationDate;
        this.licenseNumber = licenseNumber;
        this.zipcode = zipcode;
    }
}
```





-
### Brief Example, Part 2
* Issues with construction:
	* Difficult to read and identify which field corresponds to which value.
	* `null` values must be explicitly initialized

```java
public void demo() {
		License license = new License(
						"John",
						"123 Square Lane",
						null,
						"Milford",
						"Delaware",
						new Date(),
						null,
						null,
						1238913312,
						19720);
}
```



-
### Brief Example, Part 3
* Notice the setter's return-type
  * This allows setting fields to be chained
```java
public class LicenseBuilder {
    private String name;
    private String addressLine1;
    private String addressLine2;
    // ... more fields

    public LicenseBuilder setName(String name) {
        this.name = name;
        return this;
    }

		// ... more setters
    public License build() {
        return new License(name, addressLine1, addressLine2, city, state, birthDate, issuedDate, expirationDate, licenseNumber, zipcode);
    }
}
```

### Constructing a License with LicenseBuilder
* Notice the fields are initialized in arbitrary order
```java
public void demo() {
		License license = new LicenseBuilder()
            .setBirthDate(new Date()),
						.setName("John"),
						.setAddressLine1("123 Square Lane"),
						.setCity("Milford"),
						.setState("Delaware"),
            .setZipCode(19720);
						.setLicenseNumber(1238913312)
            .build();
}
```



-
-
![Image of Puppies](https://i.ytimg.com/vi/bxvoEiCas5Y/maxresdefault.jpg)
