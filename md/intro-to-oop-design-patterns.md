# Design<br>Patterns<br>and<br>Principles

-
-
# What is a Design Pattern
* A pattern that arises from the means by which a set of objects communicate.
* A recognized & established way of solving a problem by object orientation patterns.
* A named solution to a problem in a context.
* Has been proven effective, stable, and scalable.
* Thoroughly tested strategies which have been proven effective, stable, and scalable.

-
# Purpose
* Assists with the design of a system.
* Establish vocabulary which communicates a problem, solution, and potential consequences

* This is true for chess and true for programming: Raw experience can teach you enough to play a fair game, but a master studies the game of past masters; Identify patterns of play.
	* Uncle Bob

















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
* Describes the elements that make up the design, their relationships, respobsibilities, and collaborations.
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
* Purpose - Defer part of object creation to another class
* Some Examples:
	* [Singleton](http://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples#enum-singleton)
	* Factory
	* Builder


-
## Creational Patterns: Singleton
* Problem:
	* Ensure a class only has one instance and provide a global access point to it.
* Different Archetypes:
	* Eager initialization
	* Lazy initialization
	* Enum singleton
 	* Bill Pugh singleton


-
### Eager Initialization
* **Solution:** the instance of Singleton Class is created at the time of class loading
* **Consequence:** instance is created even if client application might not be using it.

```java
public class EagerSingleton {    
    private static final EagerSingleton instance = new EagerSingleton();

    private EagerSingleton(){}

    public static EagerSingleton getInstance(){
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


![Image of Puppies](https://i.ytimg.com/vi/zdcAbMwO6Zs/maxresdefault.jpg)


-
-
## Creational Patterns: Factory
* Defers some part of object construction to another class
* [Factory Method]((https://github.com/Zipcoder/TC-LectureDemo-DesignPrinciples/blob/master/src/main/java/io/zipcoder/design_demo/stage4/Main.java))
	* A method which is responsible for instantiating and returning an object.
* [Factory Pattern](https://github.com/Zipcoder/TC-LectureDemo-DesignPrinciples/blob/master/src/main/java/io/zipcoder/design_demo/stage6/Main.java)
	* A system which includes a class solely responsible for creation of another.
* Abstract Factory Class Pattern
	* Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

-
-


![Image of Puppies](https://i.ytimg.com/vi/bxvoEiCas5Y/maxresdefault.jpg)


-
-
## Creational Patterns: Builder
* Separate the construction of a complex object from its representation so that the same construction process can create different representations

* Builder pattern was introduced to solve some of the problems with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.

-

There are three major issues with Factory and Abstract Factory design patterns when the Object contains a lot of attributes.
1. Too Many arguments to pass from client program to the Factory class that can be error prone because most of the time, the type of arguments are same and from client side its hard to maintain the order of the argument.
2. Some of the parameters might be optional but in Factory pattern, we are forced to send all the parameters and optional parameters need to send as NULL.
3. If the object is heavy and its creation is complex, then all that complexity will be part of Factory classes that is confusing.


-
-
# Structural Patterns
* Handles composition of classes or objects
* Some examples:
	* Decorator
	* Adapter


-
## Structural Patterns: Decorator
* Attach additional responsibilities to an obejct dynamically
* Provides a flexible alternatie to subclassing for extending functionality.

-
## Structural Patterns: Adapter
* Converts interface of a class into another interface expected by client.
* Lets classes work together that couldn't otherwise because of incompatible interfaces



-
-
# Behavioral Patterns
* Characterize the ways in which classes or objects interact and distribute responsibility.
* Some Examples:
	* Observer
	* Command

-
## Behavioral Patterns: Observer
* Register observer
* Remove observer
* Notify observer on state change


-
## Behavioral Patterns: Command
* Command interface
	* Decoupling "what is done" from "when it is done"
	* Concrete commands objects are tasks; i.e. - `UNDO_TASK`

-
## Behavioral Patterns: Command
You'll see command being used a lot when you need to have multiple undo operations, where a stack of the recently executed commands are maintained. To implement the undo, all you need to do is get the last Command in the stack and execute it's undo() method.

You'll also find Command useful for wizards, progress bars, GUI buttons and menu actions, and other transactional behavior.  

-
-
![Image of Puppies](https://i.pinimg.com/736x/95/2a/04/952a04ea85a8d1b0134516c52198745e--rottweilers-labradors.jpg)
