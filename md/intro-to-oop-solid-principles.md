#SOLID Design Principles
-
-
## What we'll cover
<ul>
<li class="fragment fade-up"> What are Design Principles? </li>
<li class="fragment fade-up"> DRY Principle </li>
<li class="fragment fade-up"> WET Principle </li>
<li class="fragment fade-up"> SOLID Principle </li>
<li class="fragment fade-up"> Single Responsibility Principle (SRP)</li>
</ul>

-

## What we'll cover (continued)
<ul>
<li class="fragment fade-up"> Open/closed principle (OCP) </li>
<li class="fragment fade-up"> Liskov substitution principle (LSP) </li>
<li class="fragment fade-up"> Interface segregation principle (ISP)</li>
<li class="fragment fade-up"> Dependency inversion principle (DIP)</li>
</ul>
-
-

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
* The responsibility of a class should be entirely encapsulated by the class.
* The services of a class should be narrowly aligned with the responsibility of that module.
* Robert C. Martin expresses the principle as, "A class should have only one reason to change."

-
-
### Single Responsibility Principle (SRP)<br>Example
* `String` class has a single responsibility of managing an array of `Character`
* Violations of SRP enforce a _WET_ design.
	* WET design promotes an _anti-pattern_ known as _God Object Pattern_



-
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
### Open/closed principle (OCP)<br>Example
* _jarred_ projects ensure that a client can use & and compose packaged classes, but prevents client from modifying class.
* `junit.org.Test` is made available for client use & extension, but closed to modification


-
-
## Liskov substitution principle (LSP)
* **objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.**
* [Illustrating LSP](https://stackoverflow.com/questions/56860/what-is-an-example-of-the-liskov-substitution-principle)

-
### Liskov substitution principle (LSP)<br>Example (Coupled)
* Here, we have coupled a `Classroom` to a specific `List` implementation, `ArrayList`

```java
public class Classroom {
    private ArrayList<Student> students;
    public Classroom(ArrayList<Student> student) {
        this.students = students;
    }
}
```

```java
public void demo() {
	ArrayList<Student> students = new ArrayList<>();
	Classroom classroom = new Classroom(students);
}
```

-
### Liskov substitution principle (LSP)<br>Example (Decoupled)
* Here, we have decoupled a `Classroom` from a specific `List` implementation

```java
public class Classroom {
    private List<Student> students;
    public Classroom(List<Student> student) {
        this.students = students;
    }
}
```

```java
public void demo() {
	List<Student> studentArrayList = new ArrayList<>();
	List<Student> studentLinkedList = new LinkedList<>();

	Classroom classroom1 = new Classroom(studentArrayList);
	Classroom classroom2 = new Classroom(studentLinkedList);
}
```





-
-
## Interface segregation principle (ISP)
* **many client-specific interfaces are better than one general-purpose interface.**
* No client should be forced to depend on methods it does not use.
* Splits large, vague interfaces into smaller, specific ones giving the client access to only methods that are of interest to them.
* This principle should arise naturally from a project properly abiding by SRP



-
### Interface segregation principle (ISP)<br>Example Part 1
* Here, we have coupled a `Person` to an `identify` and `serialize` behavior

```java
public class Person {
	private Long id;
	private String serial;
	public Person(Long id, String serial) {
		this.id = id;
		this.serial = serial;
	}

	public Long identify() { return id; }
	public String serialize() { return serial; }
}
```


-
### Interface segregation principle (ISP)<br>Example Part 2
* If other classes are _identifiable_, or _serializable_ they have no common class; it makes most sense to create an `Identifiable` and `Serializable` interface
* This decoupling allows us to couple a single behavior at a time to a class

```java
public interface Identifiable { Long identify(); }
```

```java
public interface Serializable { String serialize(); }
```

-
### Interface segregation principle (ISP)<br>Example Part 3
* Creating an `Identifiable` person

```java
public class Person implements Identifiable {
	private Long id;
	public Person(Long id) {
			this.id = id;
	}

	public Long identify() { return id; }
}
```





-
### Interface segregation principle (ISP)<br>Example Part 4
* Creating a `Serializable` person

```java
public class Person implements Identifiable {
	private String serial;
	public Person(String serial) {
			this.serial = serial;
	}

	public String serialize() { return serial; }
}
```




-
### Interface segregation principle (ISP)<br>Example Part 5
* Creating a `Serializable` & `Identifiable` person

```java
public class Person implements Identifiable, Serializable {
	private Long id;
	public IdentifiablePerson(Long id, String serial) {
			this.id = id;
			this.serial = serial;
	}

	public Long identify() { return id; }
	public String serialize() { return serial; }
}
```

-
### Interface segregation principle (ISP)<br>Example Part 6
* The decoupling allows us to:
	* program to very specific interfaces
	* interchange different implementations of the interface

```java
public void demo() {
	Person person = new Person(0L, "Blah Blah Serial");
	Identifiable identifiable = person;
	Serializable serializable = person;
}
```



-
## Dependency inversion principle (DIP)
* **one should depend upon abstractions, not concretions**
	* A. High-level modules should not depend on low-level modules; Both should depend on abstractions.
	* B. Abstractions should not depend on details; Details should depend on abstractions.
