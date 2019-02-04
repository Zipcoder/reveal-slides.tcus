
# Object Relations


-
-
# Object Relations
| Relation Name | Verbal Expression |
|---|---|
| Association | Has reference to a |
| Aggregation | Has ownership of a |
| Composition | Has reference to a |
| Inheritance | Is a |
| Dependence  | Uses a |









-
-
## Association<br>("has reference to a")
* Indicates that a class holds a reference to another class
* Typically implemented in java through the use of an _instance field_.
* Can be bi-directional with each class holding a reference to the other.
* Is achieved through the more specific associative relations:
	* Aggregation
	* Composition











-
-
## Aggregation ("has-a")
* restricted form of **association relations**.
* denotes that an object has ownership of another object
	* Each object referenced is said to be a child of the referencer (parent).
* Is a one-way (unidirectional) relationship
* Typically implemented in java through the use of an _instance field_.



-
## Aggregation ("has-a")<br>Example
* Express that a `Person` **has a** `name` using **aggregation**.

```java
public class Person {
	private String name;
}
```









-
-
## Composition ("has-a")
* restricted form of **aggregation relations**.
* denotes that an object is responsible for the creation and destruction of another object.


-
## Composition ("has-a")
* Express that a `Person` **has a** `birthDate` using **composition**.

```java
public class Person {
	private GregorianCalendar birthDate;
	public Person(int day, int month, int year) {
	  this.birthDate = new GregorianCalendar(year, month, day);
	}
}
```









-
-
## Dependence<br>("uses-a")
* **Dependence** describes a “uses-a” relationship.
* Typically implemented in java by ensuring a method consumes a parameter-type of the dependee.


-
## Dependence<br>("uses-a")<br>Example
* Express that a `Grader` **uses a** `Student` to identify the respective `Student` grade as a `Character`.

```java
public class Grader {
	public Character grade(Student student) {
		// ... definition ommitted for brevity ...
	}
}
```












-
-
## Inheritance ("is-a")
* This relationship denotes dynamic-polymorphism.
* denotes that a class is a specific type of a more general super class.
	* denotes a class has inherent-members (methods / variables) from super class
	* inherent-members are not explicitly declared in the inheriting class (super class)
* All classes are implicitly sub-classes of the `Object` class.


-
## Inheritance ("is-a")<br>Example
* Express that a `Student` is a `Person` with a grade represented as a `Character`

```java
public class Person {
	private String name;
	private Date birthDate;
	public Person(String name, Date birthDate) {
		this.name = name;
		this.birthDate = birthDate;
	} // getters and setter ommitted for brevity
}
```

```java
public class Student {
	private Character currentGrade;
	public Student(String name, Date birthDate) {
		super(name, birthDate);
		this.currentGrade = 'F';
	} // getters and setter ommitted for brevity
}
```
