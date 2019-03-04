# Design Principles
* A set of guidelines that helps us to avoid having a bad design.

-
# DRY Principle
* Don't Repeat Yourself
* Aimed to reduce repetition of all kinds.
* Composition (black-box reuse) to inheritance (white-box reuse)
	* Inheritance exposes object internals (violation of encapsulation)
* Design to interface, not implementation

-
# WET Principle
* Violations of DRY are typically referred to as WET solutions
	* write everything twice
	* we enjoy typing
	* waste everyone's time

-
# Rule of Three
* Code refactoring rule of thumb
* states that the code can be copied once, but that when the same code is used three times, it should be extracted into a new procedure



-
-
# SOLID Principle
* Single Responsibility Principle
* Open/Closed Principle
* Liskov Substitution Principle
* Interface Segregation Principle
* Dependency Inversion Principle



-
-
## Single Responsibility Principle (SRP)
* **Every class should have responsibility over a single part of the functionality provided by the software.**
* The responibility of a class should be entirely encapsulated by the class.
* The services of a class should be narrowly aligned with the responsibility of that module.
* Robert C. Martin expresses the principle as, "A class should have only one reason to change."



-
## Open/closed principle (OCP)
* **Software entities should be open for extension, but closed for modification.**
* Polymorphic open/closed principle
* Meyer's open/closed principle
	* A class is open if it is still available for extension.
		* Open, since any new class may use it as parent, adding new features.
	* A module will be said to be closed if it is available for use by other modules.
		* Closed since it may be compiled, stored in a library, baselined, and used by client classes.

-
## Liskov substitution principle (LSP)
* **objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.**
* [Illustrating LSP](https://stackoverflow.com/questions/56860/what-is-an-example-of-the-liskov-substitution-principle)

-
## Interface segregation principle (ISP)
* **many client-specific interfaces are better than one general-purpose interface.**
* No client should be forced to depend on methods it does not use.
* Splits large, vague interfaces into smaller, specific ones giving the client access to only methods that are of interest to them.
* This principle should arise naturally from a project properly abiding by SRP

-
## Dependency inversion principle (DIP)
* **one should depend upon abstractions, not concretions**
	* A. High-level modules should not depend on low-level modules.<br>Both should depend on abstractions.
	* B. Abstractions should not depend on details.<br>Details should depend on abstractions.
