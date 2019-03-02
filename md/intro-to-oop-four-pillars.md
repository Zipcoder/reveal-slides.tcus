# Four Pillars
* Abstraction
* Polymorphism
* Inheritance
* Encapsulation


-
-
## Abstraction
* the process of exposing essential features of an entity while hiding other irrelevant detail.
* abstraction reduces code complexity and at the same time it makes your aesthetically pleasant.


-
### Abstraction
* Fetching & printing user input without abstraction
```java
public void withoutAbstraction() {
    InputStream inputStream = System.in;
    Scanner scanner = new Scanner(inputStream);
    String promptName = "What is your name?";
    String promptAge = "What is your age?";

    System.out.println(promptName);
    String stringName = scanner.nextLine();

    System.out.println(promptAge);
    String stringAge = scanner.nextLine();
    Integer intAge = Integer.parseInt(stringAge);

    System.out.println("Name = " + stringName);
    System.out.println("Age = " + intAge);
}
```



-
### Abstraction
* Fetching & printing user input with abstraction

```java
public void withAbstraction() {
    IOConsole console = new IOConsole();
    String promptName = "What is your name?";
    String promptAge = "What is your age?";
    String userInputName = console.getStringInput(promptName);
    Integer userInputAge = console.getIntegerInput(promptAge);
    Person person = new Person(userInputName, userInputAge);
    console.print(person.toString());
}
```














-
-
## Polymorphism
* ability to take on different forms or stages




-
### Polymorphism<br>Example 1
* the ability to take on different forms or stages

```java
public interface Flyer { void fly(int distance); }

public abstract class Animal { String speak(); }

public class Bird extends Animal implements Flyer {
  @Override
  public void fly(int distance) {
    this.setLocation(getX(), getY()+distance);
  }

  @Override
  public String speak() {
    return "chirp!"
  }
}
```


-
### Static Polymorphism<br>Example 2
* Also known as compile-time polymorphism.
```java
public void demo() {
}
```


-
### Runtime Polymorphism<br>Example 3
* Also known as dynamic-time polymorphism.
```java
public void demo() {
}
```



























-
-
## Inheritance
* blah blah


-
### Inheritance<br>Example 1
```java
public void demo() {
}
```


-
### Inheritance<br>Example 2
```java
public void demo() {
}
```




-
### Inheritance<br>Example 3
```java
public void demo() {
}
```









































-
-
## Encapsulation
* modules, internal data, and other implementation details/mechanism are hidden or _inaccessible_ from other modules
* results in as data-hiding


-
### Encapsulation<br>Example 1
```java
public void demo() {
}
```


-
### Encapsulation<br>Example 2
```java
public void demo() {
}
```




-
### Encapsulation<br>Example 3
```java
public void demo() {
}
```
