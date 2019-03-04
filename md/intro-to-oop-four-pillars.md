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
* Dynamic (run-time) polymorphism: JVM, rather than compiler, decides which method to call
* Static (compile-time) polymorphism: compiler identifies method to call by method-signature

-
### Dynamic Polymorphism
* Also known as _run time_ polymorphism
* The Java compiler does not understand which method to call, the decision is deferred to JVM at run-time.
* _Method overriding_ of instance methods is an example of dynamic polymorphism in Java




-
### Dynamic (Runtime) Polymorphism<br>Designing Bird class
* The following design enforces polymorphic behavior of a `Bird`

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
### Dynamic (Runtime) Polymorphism<br>Bird Example
* `Bird` as `Animal` exposes only `speak` method.

```java
public void demo() {
  Animal animal = new Bird();
  animal.speak();
}
```


-
### Dynamic (Runtime) Polymorphism<br>Bird Example
* `Bird` as `Flyer` exposes only `fly` method.

```java
public void demo() {
  Flyer flyer = new Bird();
  flyer.fly(10);
}
```


-
### Dynamic (Runtime) Polymorphism<br>Bird Example
* `Bird` as `Bird` exposes `speak` and `fly` method.

```java
public void demo() {
  Bird bird = new Bird();
  bird.speak();
  bird.fly(10);
}
```
















-
### Static Polymorphism
* Also known as _compile-time polymorphism_ or _static binding_
* At compile time, Java knows which method to invoke by checking the method signatures.










-
### Static (compile-time) Polymorphism<br>Designing Person class
* The following design enforces polymorphic behavior of a `Person` by overloading the constructor

```java
public class Person {
  private String name;
  private Integer age;
  private Color eyeColor;

  public Person(String name, Integer age, Color eyeColor) {
    this.name = name;
    this.age = age;
    this.eyeColor = eyeColor;
  }

  public Person() {
    this("John", 50, Color.BLACK);
  }
}
```


-
### Static (compile-time) Polymorphism<br>Person Example
* _Primary_ constructor of `Person` class

```java
public void demo() {
  String personName = "Douglas";
  Integer personAge = 25;
  Color personEyeColor = Color.BLUE;
  Person person = new Person(personName, personAge, personEyeColor);

  System.out.println(person.getName());
  System.out.println(person.getAge());
  System.out.println(person.getEyeColor());
}
```

Output
```java
Douglas
25
BLUE
```





















-
-
## Inheritance
* the process where one class acquires the members (methods and fields) of another.
* Ensures information is made manageable in a hierarchical order.


-
### Inheritance<br>Designing an Animal class
* the class whose properties are inherited is known as _superclass_ (base-class).
  * Here, the `Animal` class is the intended to be the superclass.
```java
public class Animal {
  protected Point position;

  public Animal(Integer xPosition, Integer yPosition) {
    this.position = new Point(xPosition, yPosition);
  }

  public void setPosition(Integer xPosition, Integer yPosition) {
    this.position = new Point(xPosition, yPosition);
  }

  public Point getPosition() {
    return this.position;
  }
}
```


-
### Inheritance<br>Designing a Fox class
* The class which inherits the members of other is known as _subclass_ (derived class)
  * Here, the `Fox` class inherits members from `Animal`
```java
public class Fox extends Animal {
  public Fox(Integer xPosition, yPosition) {
    super(xPosition, yPosition);
  }
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
