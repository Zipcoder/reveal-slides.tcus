
# Object Relations


-
-
# Object Relations
| Relation Name | Verbal Expression |
|---|---|---|
| Association | Has reference to a |
| Aggregation | Has ownership of a |
| Composition | Has ownership of a |
| Inheritance | Is a |
| Dependence  | Uses a |









-
-
## Association<br>("has-reference-to-a")
* Indicates that a class holds a reference to another instance of a class.
* In Java, it can be implemented through the use of an _instance field_.
* Can be bi-directional with each class holding a reference to the other.
* Is achieved through the more specific associative relations:
	* Aggregation
	* Composition











-
-
## Aggregation<br>"has-a" or "has many"
* restricted form of **association relations**.
* denotes that an object has ownership of another object
	* Each object referenced is said to be a child of the referencer (parent).
* In Java, can be implemented through the use of an _instance field_.
* Is a one-way (unidirectional) relationship
* The _degree of cardinality_ can be
	* **one to one** (`1-1`)
	* **one to many** (`1-M`)



-
## Aggregation ("has-a")<br>Example
* Express that a `Person` **has a** `pet` using **aggregation**.

```java
public class Person {
	private Animal pet;
}
```


-
## Aggregation ("has-many")<br>Example 1
* Express that a `Person` **has many** `pets` using **aggregation**.

```java
public class Person {
	private Animal[] pets;
}
```



-
## Aggregation ("has-many")<br>Example 2
* Express that a `Person` **has many** `pets` using **aggregation**.

```java
public class Person {
	private List<Animal> pets;
}
```
















-
-
## Composition ("has-a")
* restricted form of **aggregation relations**.
* denotes that an object is responsible for the creation AND destruction of another object.
	* when `this` object is garbage collected, its _composite children_ are also garbage collected
* The _degree of cardinality_ can be
	* **one to one** (`1-1`)
	* **one to many** (`1-M`)


-
## Composition ("has-a")<br>Example
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
* Express that a `Grader` **uses a** `Student` to obtain a `Character` representative of a grade.

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
	* denotes a class has **inherent**-members (methods / variables) from super class
	* inherent-members are not explicitly declared in the inheriting class (sub class)
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
public class Student extends Person {
	private Character currentGrade;
	public Student(String name, Date birthDate, Character initialGrade) {
		super(name, birthDate);
		this.currentGrade = initialGrade;
	} // getters and setter ommitted for brevity
}
```
