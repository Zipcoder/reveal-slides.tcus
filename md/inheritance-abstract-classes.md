# Inheritance

-
## Abstract classes

-
### What is an Abstract Class?
* A class which cannot be instantiated.
* Is used to couple subclasses to common construction & behaviors
* Is used by creating an inheriting subclass that can be instantiated.
* An abstract class does a few things for the inheriting subclass:
  1. Define methods which can be used by the inheriting subclass.
  2. Define abstract methods which the inheriting subclass must implement.
  3. Provide a common interface which allows the subclass to be interchanged with all other subclasses.




-
### Deciding on Declaring a class Abstract
* Should be created when subclasses must be coupled to common construction of an entity which should never be instantiated.
  * Should be created when subclasses must be coupled to common behavior, we create an interface.
* Should be created when an entity should be instantiable, but contains data-members which should be used by subclasses.



-
### Defining Abstract Classes
```java
abstract public class Shape {
  private double width;
  private double height;
  public Shape(double width, double height) {
    this.width = width;
    this.height = height;
  }
  // getters & setters ommitted
}
```




-
### Deciding on declaring a method Abstract
* An abstract method is
* An abstract method should be created when an entity should be instantiable, but contains data-members which should be used by subclasses.


-
### Declaring Abstract Method
* Methods which are _declared_, are abstract.

```java
abstract public class Shape {
  abstract public Double getArea();
}
```


-
### Declaring Abstract Method
* Interface method-declarations are implicitly _abstract_.

```java
public interface Shape {
  Double getArea();
}
```

-
### Providing implementation for Abstract Method
* Methods which are _defined_, are not abstract.


```java
public class Square extends Shape {
  public Square(double length) {
    super(length, length)
  }

  @Override
  public Double getArea() {
    return Math.pow(length, 2);
  }
}
```


-
### Declaring a Final Class
* Prevents class-hierarchy from deepening.
* Declare a class `final` when it should not be subclassed (i.e. - making it closed to modification)
* `String` is a `final` class.



-
### Declaring a Final Class Examples

```java
public final class Square extends Shape {
  public Square(double length) {
    super(length, length)
  }
  public Double getArea() {
    return Math.pow(length, 2);
  }
}
```


-
### Final Methods
* Making a method `final` prevents other sub-classes from overriding the method.
