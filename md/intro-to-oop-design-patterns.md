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
# Structural Patterns
* Concerned with how classes and objects are composed to form larger structures
* Structural _class_ patterns use inheritance to compose interfaces or implementations.
* Some examples:
	* Decorator
	* Adapter
	* Proxy


-
-
# Behavioral Patterns
* Characterize the ways in which classes or objects interact and distribute responsibility.
* Concerned with algorithms and the assignment of responsibilities between objects
* Describes object / class patterns as well as the resulting communication patterns between them
* Some Examples:
	* Observer
	* Command


-
-
![Image of Puppies](https://i.pinimg.com/736x/95/2a/04/952a04ea85a8d1b0134516c52198745e--rottweilers-labradors.jpg)
