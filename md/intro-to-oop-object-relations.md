# Object Relations
* "Is assosicated with"; association
* "Has a"; aggregation / composition
* "Is a"; inheritance / polymorphism
* "Uses a"; dependence


-
-
## Association
* Association a connection between two separate classes which sets up through their Objects.
* Although, Java association can balance, one-to-many, many-to-one, and many-to-many.

-
## Association
* Now, Aggregation in Java is a special type of association. Java Aggregation has the following characteristics
	* A has-A relationship is represented here.
	* It is a one-way relationship, i.e. a unidirectional relationship.
	* Ending one entity won’t affect another, both can be present independently.
-
-
## Aggregation ("has-a")
* denotes a has-a relationship.
* one-way (unidirectional) relationship
* Ending one entity won’t affect another, both can be present independently.


-
## Aggregation ("has-a")
* Programs by nature should avoid complexity, so keep things as simple as possible.
* The objects that you create, and the objects that you use should have a SINGLE-RESPONSIBLITY.
	* They should have one task to do, and do it well.
* Complexity is achieved by creating container classes, which are comprised of simple objects working together to achieve one objective.


-
-
## Composition ("has-a")
* restricted form of aggregation.
* quantities are highly dependent on each other.
* represents a part-of relationship.
* One entity cannot exist without the other.


-
## Inheritance ("is-a")
* This relationship is polymorphic in nature.
* this relationship describes what the object is extended from.
* Remember that all objects in java are extended from `Object` class.


-
-
# Dependence Relation
* **Dependence** describes a “uses-a” relationship.
* Think about the objects a class needs to complete its job.
	* Different than "has a" because

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
