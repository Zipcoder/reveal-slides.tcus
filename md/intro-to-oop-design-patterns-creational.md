# Design patterns



-
-

## What we'll cover
<ul>
	<li class="fragment fade-up">What is a Creational Design Pattern?</li>	
	<li class="fragment fade-up">Recurring Themes of Creational Patterns
		<ul>
			<li class="fragment fade-up">Singleton Pattern</li>
			<li class="fragment fade-up">Factory Pattern</li>
			<li class="fragment fade-up">Builder Pattern</li>
		</ul>
	</li>
</ul>







-
-

## Creational Patterns
* Defers part of object creation to another class
* Some Examples:
	* Singleton
	* Factory
	* Builder


-
### Two Recurring Themes of Creational Patterns
<ol>
<li class="fragment fade-up">They encapsulate knowledge about which concrete classes the system uses
	<ul><li class="fragment fade-up">Abstracts the instantiation process</li></ul>
	</li>
<li class="fragment fade-up">Hide how instances of classes are created and put together
	<ul>
		<li class="fragment fade-up">Helps make a system independent of how its objects are created, composed, or represented</li>
	<ul></li>
</ol>




-
-
### Singleton Pattern
* Problem - Ensure a class only has one instance and provide a global access point to it.

-
### Brief Example
* Consider a Casino application wherein a `Player` instance requires a reference to an _externally persistent_ `Profile`.

```java
public class GoFishGame {
  private List<GoFishPlayer> playerList;

  public void addNewPlayer() {
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

  public void addNewPlayer() {
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
    if(instance == null) {
      instance = new ProfileManager();
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

  public void addNewPlayer() {
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

  public void addNewPlayer() {
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

<ul>
	<li class="fragment fade-up">Factory Method
		<ul>
			<li class="fragment fade-up">A method which is responsible for instantiating and returning an object.</li>
		</ul>
	</li>
	<li class="fragment fade-up">Abstract Factory Class Pattern
		<ul>
			<li class="fragment fade-up">Provides an interface for creating families of related or dependent objects without specifying their concrete classes.</li>
		</ul>
	</li>
	<li class="fragment fade-up">Factory Pattern
		<ul>
			<li class="fragment fade-up"><span style="    font-style: italic;">Informal pattern</span> in which there exists a class solely responsible for creation of another.</li>
		</ul>
	</li>
</ul>

-
### Factory Methods; Example 1
* Each `String`-returning method in the `String` class is a _factory method_.
  * Each of such methods returns a new instance of a `String`

```java
public void demo() {
  String string = "Hello world";
  string.replaceAll("Hello", "");
  System.out.println(string);
}
```

Output
```java
Hello world
```

-
### Factory Methods; Example 2
* Here, the `PersonFactory` acts as a _factory class_ and the `createRandomPerson` method acts as a _factory method_.

```java
public class PersonFactory {
  // factory method
  public Person createRandomPerson() {
    String name = RandomUtils.createString('a', 'z', 5);
    return createRandomlyAgedPerson(name);
  }

  // factory method
  public Person createRandomlyAgedPerson(String name) {
    Integer randomAge = RandomUtils.createInteger(0, 100);
    return new Person(name, randomAge);
  }
}
```

-
## Abstract Factory Pattern Example
* Read the book...<br>(start at 4.4.4)


-
### Issues with Factory Pattern
<ul>
	<li class="fragment fade-up">Too many arguments to pass from client program to the Factory class that can be error prone
		<ul>
	  		<li class="fragment fade-up"> Often, argument-types are same. From client-side, it's hard to maintain the order of the argument.</li>
	  </ul>
  </li>
  <li class="fragment fade-up">Factory pattern does not do optional parameters well.
	  <ul>
	  	<li class="fragment fade-up">We are forced to send <strong>all</strong> the parameters, and optional parameters need to send as NULL.</li>
	  </ul>
  </li>
  </ul>











-
-
## Creational Patterns: Builder
<ul>
	<li class="fragment fade-up">Problem: Separate the construction of a complex object from its representation, so that the same construction process can create different representations</li>
	<li class="fragment fade-up">Resolves issues with large construction involving a lot of attributes.</li>
</ul>


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

    public License(String name, String addressLine1,
                   String addressLine2, String city,
                   String state, Date birthDate,
                   Date issuedDate, Date expirationDate,
                   Integer licenseNumber, Integer zipcode) {
       // definition omitted
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

-
### Constructing a License with LicenseBuilder
* Notice the fields are initialized in arbitrary order
* Notice that `null` values are ignored

```java
public void demo() {
		License license = new LicenseBuilder()
            .setBirthDate(new Date())
            .setName("John")
            .setAddressLine1("123 Square Lane")
            .setCity("Milford")
            .setState("Delaware")
            .setZipCode(19720)
            .setLicenseNumber(1238913312)
            .build();
}
```



-
-
![Image of Puppies](https://i.ytimg.com/vi/bxvoEiCas5Y/maxresdefault.jpg)
