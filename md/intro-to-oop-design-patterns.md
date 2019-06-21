#Design<br>Patterns<br>and<br>Principles

-
-

## What we'll cover
<ul>
	<li class="fragment fade-up"> What is a Design Pattern? </li>
	<li class="fragment fade-up"> Four Essential Elements of a Pattern</li>
	<li class="fragment fade-up"> Classifying Design Patterns </li>
	<li class="fragment fade-up"> Brief introduction to: 


	<ul>
		<li class="fragment fade-up"> Creational Patterns</li>
		<li class="fragment fade-up"> Structural Patterns</li>
		<li class="fragment fade-up"> Behavioral Patterns</li>
	</ul>
 </li>
</ul>


-
-

# What is a Design Pattern?
<ul>
<li class="fragment fade-up"> A pattern that arises from the means by which a set of objects communicate.</li>
<li class="fragment fade-up"> A recognized & established way of solving a problem by object orientation patterns.</li>
<li class="fragment fade-up"> A named solution to a problem in a context.</li>
<li class="fragment fade-up"> Has been proven effective, stable, and scalable.</li>
<li class="fragment fade-up"> Thoroughly tested strategies which have been proven effective, stable, and scalable.</li>
</ul>


-
# Purpose of a Design Pattern
<ul>
<li class="fragment fade-up">  Assists with the design of a system.</li>
<li class="fragment fade-up"> Establish vocabulary which communicates a problem, solution, and potential consequences</li>

<li class="fragment fade-up"> This is true for chess and true for programming: Raw experience can teach you enough to play a fair game, but a master studies the game of past masters; Identify patterns of play.
	<ul><li class="fragment fade-up"> Uncle Bob</li></ul></li>
</ul>


















-
-
# Four Essential Elements of a Pattern
1. Pattern Name
2. Problem
3. Solution
4. Consequences

-
# 1. Pattern name
* a word or two we can use to describe a design
	* problem
	* solution
	* consequence

* Naming increases design vocabulary
* Enables discussion and design at a higher level of abstraction.
* "Bisected Oval Pattern"

-
# 2. Problem
* Describes when to apply pattern
* Explains problem and its context
* Sometimes problems will include a list of conditions that must be met before it makes sense to apply the pattern.
* "Is our subject facing forward?"

-
# 3. Solution
* Describes the elements that make up the design, their relationships, responsibilities, and collaborations.
* Provides an abstract description of a design problem and how a general arrangement of elements solves it.
* "_Oval bisection_ allows early planning for placement of facial features"

-
# 4. Consequences
* The results and trade-offs of applying the pattern.
* Address memory, time, and language implementation issues.
* Include impact on a system's flexibilitiy, extensibility, portability.
* "Yields a forward facing portrait. Does not support profile portraits."













-
-
# Classifying Design Patterns
* 3 Major types of design patterns:
	* Creational - Defer part of object creation to another class
	* Structural - Composition of classes or objects
	* Behavioral - Characterize the ways in which classes or objects interact and distribute responsibility

























-
-
# Creational Patterns
* Defers part of object creation to another class
* Some Examples:
	* [Singleton](http://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples#enum-singleton)
	* Factory
	* Builder


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





-
-
## Creational Patterns: Factory
* Problem - Separate the construction of a complex object from its representation so that the same construction process can create different representations

-
### Brief Example
* Here, the `LicenseBuilder` is a _builder_ of `License` objects.
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
## Creational Patterns: Builder
* Problem - Separate the construction of a complex object from its representation so that the same construction process can create different representations

-
### Brief Example
* Here, the `LicenseBuilder` is a _builder_ of `License` objects.
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
## Creational Patterns: Factory
* Create a class solely responsible for creation instances of another.

-
### Brief Example
* Here, the `PersonFactory` acts as a _factory class_ and the `createRandomPerson` method acts as a _factory method_.

```java
public class PersonFactory {
  // factory method
  public Person createRandomPerson() {
    return createRandomlyAgedPerson(RandomUtils.createInteger(0, 100));
  }

  // factory method
  public Person createRandomlyAgedPerson(String name) {
    Integer randomAge = RandomUtils.createInteger(0, 100);
    return new Person(name, randomAge);
  }
}
```













-
-
# Structural Patterns
* Concerned with how classes and objects are composed to form larger structures
* Structural _class_ patterns use inheritance to compose interfaces or implementations.
* Some examples:
	* Decorator
	* Adapter
	* Proxy

-
## Brief Example
* Ensures that a _new instance_ of type `T` wraps the behavior of a _pre-existing_ instance of type `T`.
	* This allows the _pre existing_ instance to gain functionality during run-time rather than creating an explicit subclass.

```java
public void demo() {
	CasinoPlayerProfile player = ProfileManager.getCurrentPlayer();
	CasinoPlayerProfile blackJackPlayer = new BlackJackPlayer(player);
	blackJackPlayer.increaseProfileBalance(100);
}
```


-
-
## Behavioral Patterns
* Characterize the ways in which classes or objects interact and distribute responsibility.
* Concerned with algorithms and the assignment of responsibilities between objects
* Describes object / class patterns as well as the resulting communication patterns between them
* Some Examples:
	* Template
	* Observer
	* Command

-
### Template Pattern
* Create a method of high freedom to be used by a method of lower freedom
	* The degree of freedom is determined by the number of arguments passed into the method


-
### Example

```java
// degree of 1
public static Integer[] getRange(int start) {
    return getRange(0, start, 1);
}

// degree of 2
public static Integer[] getRange(int start, int stop) {
    return getRange(start, stop, 1);
}

// template method; degree of 3
public static Integer[] getRange(int start, int stop, int step) {
    List<Integer> list = new ArrayList<>();
    for(int i = start; i<stop; i+=step) {
        list.add(i);
    }
    return list.toArray(new Integer[list.size()]);
}
```

-
-
![Image of Puppies](https://i.pinimg.com/736x/95/2a/04/952a04ea85a8d1b0134516c52198745e--rottweilers-labradors.jpg)
