### Abstract classes

- Cannot be instantiated
- Can contain abstract methods
- Can extend other abstract classes

-
#### Abstract methods

A method whose signature is defined, but implementation is not

```Java
abstract class Worker{

  // All workers can doWork, but some do it differently than others
  public WorkProduct doWork();
}

```

-
#### Extending Abstract Classes

- Inheritance works the same as concrete classes
- All abstract methods are inherited
- Abstract methods can be implemented
- Concrete classes must implement all remaining abstract methods

-
### Interfaces vs abstract classes

- Interfaces can be multiply implemented
- Abstract classes have state and private fields and methods available
- Concrete classes extend abstract classes; they implement interfaces (syntax difference)
- Abstract classes can implement interfaces; the reverse is not allowed
