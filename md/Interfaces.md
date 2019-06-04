# Interfaces

-
-
## What we'll cover

<p class="fragment fade-up">What is an Interface?</p>
<p class="fragment fade-up">Declaring an Interface</p>
<p class="fragment fade-up">Implementing an Interface</p>
<p class="fragment fade-up">Converting to an Interface Type</p>


-
## What we'll cover (continued)

<p class="fragment fade-up">Casting and instanceof</p>
<p class="fragment fade-up">Extending Interfaces</p>
<p class="fragment fade-up">Constants</p>
<p class="fragment fade-up">Static and Default Methods</p>


-
-
## Interfaces

-
* An interface is a contract that all classes implementing it must follow.
* It means that code can be written against the interface without worrying about what an actual object will do with it.
* Couples a class to a behavior



-
## Comparable Interface
* Consider the `Comparable` interface.


```java
public interface Comparable<T> {
  public int compareTo(T o);
}
...
public class Something implements Comparable<Something> {
  public int someInt;
  public Something(int someInt) {
    this.someInt = someInt;
  }
  public int compareTo(Something other) {
    return this.someInt - other.someInt;
  }
}
```

-
## Declaring an interface

```java
public interface SomeInterface {
  boolean someBoolFunc();
  Object funcReturnsAnObject();
  ReturnType functionName();
}
```

-
```java
public static void someFunction(SomeInterface something) {
  if(something.someBoolFunc()) {
    Object o = something.funcReturnsAnObject();
    // do something with o
  }
}
```

-
## Implementing an interface

-
```java
public interface SomeInterface {
  boolean someBoolFunc();
  Object funcReturnsAnObject();
  ReturnType functionName();
}
```
```java
public class SomeClass implements SomeInterface {
  public SomeClass() { }

  public boolean someBoolFunc() { return true; }

  public Object funcReturnsAnObject() { return new Object(); }

  public ReturnType functionName() {
    // Must return some ReturnType
  }
}
```

-
## Converting to an Interface Type

> List<String> someList = new ArrayList<>();

`List` is the supertype of `ArrayList`

`ArrayList` is the subtype of `List`

This lets us swap out code implementations easily.

-
## Casting and `instanceof`
You can cast a supertype to a subtype if you are certain that the object really is of that subtype.

If it isn't the right type, you'll get a compile-time error or a class cast exception.

We can, however, write some code to make sure that the cast will be okay using `instanceof`

-

```java
public interface SomeInterface { boolean someBoolFunc(); }
```
```java
public class Implementer implements SomeInterface {
  public Implementer() { }

  public boolean someBoolFunc() { return true; }

  public void someOtherFunc() {
    System.out.println("Some Other Function");
  }
}
```
```java
public void demo() {
  SomeInterface something = // something;
  if(something instanceof Implementer) {
    Implementer letsUseIt = (Implementer) something;
    something.someOtherFunc();
  } else {
    // not an instance of Implementer
  }
}
```

-
## Extending Interfaces
You can extend interfaces.  This means that you can have interfaces using other interfaces.

Any class implementing an extended interface must implement all of the methods from ALL the interfaces.

-
```java
public interface SomeInterface {
  boolean someBoolFunc();
}
```
```java
public interface ExtendoInterface extends SomeInterface{
  int someIntFunc();
}
```
```java
public class Usage implements ExtendoInterface {
  public Usage() {}

  public int someIntFunc() { return 42; }

  public boolean someBoolFunc() {return true; }
}
```


-
## Implementing Multiple Interfaces

```java
public class Something implements
      AnInterface,
      AnotherInterface,
      ThirdInterface {}
```

-
## Constants

All variables in an interface become `public static final`.

-
-
## Static and Default Methods

-
* Added in Java 8
* Used to have to be in a companion class
* `Collection` vs `Collections`
* Allows for interface evolution
  * This means that when new things are added, by adding a default method the older code will still work fine.

-
```java
public interface Example {
  default boolean isTrue() {
    return true;
  }
  public static boolean isEven(int n) {
    return n % 2 == 0;
  }
}
```

-
## Resolving Default Method Conflicts

```java
public interface FirstExample {
  default boolean isTrue() { return true; }
}
```
```java
public interface SecondExample {
  default boolean isTrue() { return false; }
}
```
```java
public class Example implements FirstExample, SecondExample {
  // must have
  public boolean isTrue() { return true; }
}
```
> error: class Example inherits unrelated defaults for isTrue() from types FirstExample and SecondExample

-
-
## Examples
```java
public class Something implements Comparable<Something> {
  public int someInt;
  public Something(int someInt) {
    this.someInt = someInt;
  }
  public int compareTo(Something other) {
    return this.someInt - other.someInt;
  }
}
```

-
* Comparator<T>
* Runnable
* EventHandler<T>
* Serializable
* Cloneable
