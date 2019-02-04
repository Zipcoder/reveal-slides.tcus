
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
* restricted form of association.
* In technical terms it denotes that a class has ownership of another class
	* Each class referenced is said to be a child of the referencer (parent).
* Is a one-way (unidirectional) relationship
* Typically implemented in java through the use of an _instance field_.


-
-
## Aggregation ("has-a")<br>Example
* Express that a `Person` **has a** `name`.
```java
public class Person {
	private String name;
}
```


-
-
## Composition ("has-a")
* restricted form of aggregation.
* quantities are highly dependent on each other.
* represents a part-of relationship.
* One entity cannot exist without the other.


-
## Composition ("has-a")
```java
public class Student {
  private String name;
  private GregorianCalendar dateOfBirth;
  public Student(String name, int day, int month, int year)
  {
    this.name = name;
    this.dateOfBirth = new GregorianCalendar(year, month, day);
  }
//rest of Student class..
}
```

-
## Inheritance ("is-a")
* This relationship is polymorphic in nature.
* this relationship describes what the object is extended from.
* Remember that all objects in java are extended from `Object` class.


-
-
# Dependence<br>("uses-a")
* **Dependence** describes a “uses-a” relationship.
* Typically implemented in java by ensuring a method consumes a parameter-type of the dependent.

-
## Relationships between Classes
* Objects are containers. Each has its own "state"
* Each employee works slightly different hours each week.
* But they all "punch in" on a single time-clock on the wall each with a different time-card.
* How can we model this?
* Think about a scenario in which you have a time-keeping application that has the following objects:
	* Employee
	* TimeCard
	* TimeClock

-

## Relationships between Classes
* For the TimeCard object to function properly, it needs the TimeClock object to be created and in scope.
* Employee and TimeClock can exist and function without knowledge of TimeCard.
