# Design patterns

## Creational Patterns


-
### Two Recurring Themes of Creational Patterns
1. They encapsulate knowledge about which concrete classes the system uses
	* Abstracts the instantiation process
2. Hide how instances of classes are created and put together
	* Helps make a system independent of how its objects are created, composed, or represented





-
-
### Singleton Pattern
* Problem:
	* Ensure a class only has one instance and provide a global access point to it.

-
### Brief Example
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
-
## Implementing a Singleton
* Singleton allows you to design a class as if its use will be non-static (implementing interfaces, extending classes), but accessing it gives the appearance of static operations


-
### Eager Initialization
* **Solution:** the instance of Singleton Class is created at the time of class loading
* **Consequence:** instance is created even if client application might not be using it.

-
### Eager Initialization Example
```java
public class ProfileManager implements Container<Profile> {
  private static final ProfileManager instance = new ProfileManager();
  private final Group<Profile> profileContainer;

  private ProfileManager(){ this.profileContainer = new Group<>(); }

  public static ProfileManager getInstance(){ return instance; }
  public void add(Profile profile) { profileContainer.add(profile); }
  public void remove(Profile profile) { profileContainer.remove(profile); }
  public Profile getById(Long id) { profileContainer.getById(id);}
}
```

-
### Accessing Eager Initialization singleton

```java
public class GoFishGame {
  private List<GoFishPlayer> playerList;

  public void createPlayer() {
    String userPrompt = "What is your profile ID?";
    IOConsole console = new IOConsole();

    // static `getInstance` access
    ProfileManager profileManager = ProfileManager.getInstance();

    Integer profileId = console.getIntegerInput(userPrompt);
    Profile profile = profileManager.getById(profileId);
    GoFishPlayer player = new GoFishPlayer(profile);
    playerList.add(player);
  }
}
```

-
### Lazy Initialization Example
* **Solution:** instance is not created until global access method is accessed

```java
public class ProfileManager implements Container<Profile> {
  private static final ProfileManager instance;
  private final Container<Profile> profileContainer;

  private ProfileManager(){ this.profileContainer = new Container<>(); }

  public static ProfileManager getInstance(){
    if(instance != null) {
      instance = new ProifleManager();
    }
    return instance;
  }
  public void add(Profile profile) { profileContainer.add(profile); }
  public void remove(Profile profile) { profileContainer.remove(profile); }
  public Profile getById(Long id) { return profileContainer.getById(id);}
}
```




-
### Accessing Lazy Initialization singleton

```java
public class GoFishGame {
  private List<GoFishPlayer> playerList;

  public void createPlayer() {
    String userPrompt = "What is your profile ID?";
    IOConsole console = new IOConsole();

    // static `getInstance` access
    ProfileManager profileManager = ProfileManager.getInstance();

    Integer profileId = console.getIntegerInput(userPrompt);
    Profile profile = profileManager.getById(profileId);
    GoFishPlayer player = new GoFishPlayer(profile);
    playerList.add(player);
  }
}
```


-
### Enum Singleton Example
* **Solution:** Java ensures that any enum value is instantiated only once.
* **Consequence:** does not allow lazy initialization.

```java
public enum ProfileManager implements Container<Profile> {
  INSTANCE;
  private final Container<Profile> profileContainer;

  private ProfileManager(){ this.profileContainer = new Container<>(); }

  public void add(Profile profile) { profileContainer.add(profile); }
  public void remove(Profile profile) { profileContainer.remove(profile); }
  public Profile getById(Long id) { return profileContainer.getById(id);}
}
```



-
### Accessing Enum singleton

```java
public class GoFishGame {
  private List<GoFishPlayer> playerList;

  public void createPlayer() {
    String userPrompt = "What is your profile ID?";
    IOConsole console = new IOConsole();

    // static `INSTANCE` access
    ProfileManager profileManager = ProfileManager.INSTANCE;

    Integer profileId = console.getIntegerInput(userPrompt);
    Profile profile = profileManager.getById(profileId);
    GoFishPlayer player = new GoFishPlayer(profile);
    playerList.add(player);
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
