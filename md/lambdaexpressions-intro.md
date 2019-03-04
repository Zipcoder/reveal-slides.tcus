# Lambda Expressions








-
-
## Functional Interfaces


-
## What is a Functional Interface?
* A way for java8 to handle lambda expressions & method references.
* The `@FunctionalInterface` annotation can only be placed on an interface declaring exactly 1 method.
* Can be expressed as a lambda expression or method reference.



-
# Relevant Jargon
* A `lambda` is a function which can be created without belonging to any class.
* A `method reference` is how java handles the nuance of passing methods as arguments.








-
### Relevant Functional Interfaces
| Name       | Has Return-Value | Takes Argument 1 | Takes Argument 2 |
|------------|---------|------------------|------------------|
| Runnable  | No      | No       | No      |
| Supplier   | Yes     | No       | No      |
| Consumer   | No      | Yes      | No      |
| BiConsumer | No      | Yes      | Yes     |
| Function   | Yes     | Yes      | No      |
| BiFunction | Yes     | Yes      | Yes     |


-
### Relevant Functional Interfaces
* A `Runnable` is no-argument, void-returning.
* A `Function` is single-argument, non-void-returning.
* A `Predicate` is single-argument, boolean-returning.
* A `Consumer` is single-argument, void-returning.
* A `Supplier` is no-argument, non-void-returning.
* A `BiConsumer` is two-argument, void-returning.
* A `BiFunction` is two-argument, non-void-returning.












-
-
## Functional Interface
* `Runnable` as instance method reference

```java
public class MainClass {
  public void demo() {
    Runnable runnable = this::someRunnableMethod;
    runnable.run(); // invokes method
  }

  public void someRunnableMethod() {
      System.out.println(new StringBuilder()
              .append("I don't take any arguments and ")
              .append("I don't return anything!")
              .toString());
  }
}
```


-
## Functional Interface
* `Runnable` as static method reference

```java
public class MainClass {
  public void demo() {
    Runnable runnable = MainClass::someRunnableMethod;
    runnable.run(); // invokes method
  }

  public static void someRunnableMethod() {
      System.out.println(new StringBuilder()
              .append("I don't take any arguments and ")
              .append("I don't return anything!")
              .toString());
  }
}
```






-
## Functional Interface
* `Runnable` as lambda expression

```java
public class MainClass {
  public void demo() {
    Runnable runnable = () -> {
      System.out.println(new StringBuilder()
              .append("I don't take any arguments and ")
              .append("I don't return anything!")
              .toString());
    }
    runnable.run(); // invoke method
  }
}
```



















-
-
## Functional Interface
* `Supplier` as instance method reference

```java
public class MainClass {
  public void demo() {
    Supplier<String> supplier = this::someSupplierMethod;
    String result = supplier.get();
  }

  public String someSupplierMethod() {
      return new StringBuilder()
              .append("I don't take any arguments and ")
              .append("I return a String!")
              .toString();
  }
}
```


-
## Functional Interface
* `Supplier` as static method reference

```java
public class MainClass {
  public void demo() {
    Supplier<String> supplier = MainClass::someSupplierMethod;
    String result = supplier.get();
  }

  public static String someSupplierMethod() {
      return new StringBuilder()
              .append("I don't take any arguments and ")
              .append("I return a String!")
              .toString();
  }
}
```






-
## Functional Interface
* `Supplier` as lambda expression.

```java
public class MainClass {
  public void demo() {
    Supplier<String> supplier = () -> new StringBuilder()
            .append("I don't take any arguments and ")
            .append("I return a String!")
            .toString();
    String result = supplier.get();
  }
}
```















-
-
## Functional Interface
* `Consumer` as instance method reference

```java
public class MainClass {
  public void demo() {
    Consumer<String> consumer = this::someConsumerMethod;
    String result = consumer.accept("Hello world");
  }

  public void someConsumerMethod(String str) {
    System.out.println(str);
  }
}
```


-
## Functional Interface
* `Consumer` as static method reference

```java
public class MainClass {
  public void demo() {
    Consumer<String> consumer = MainClass::someConsumerMethod;
    String result = consumer.accept("Hello world");
  }

  public static void someConsumerMethod(String str) {
    System.out.println(str);
  }
}
```






-
## Functional Interface
* `Consumer` as lambda expression

```java
public class MainClass {
  public void demo() {
    Conusmer<String> consumer = (str) -> System.out.println(str);
    consumer.accept("Hello world");
  }
}
```

























-
-
## Functional Interface
* `BiConsumer` as instance method reference

```java
public class MainClass {
  public void demo() {
    BiConsumer<String> consumer = this::someConsumerMethod;
    String result = consumer.accept("Hello", "World");
  }

  public void someConsumerMethod(String str1, String str2) {
    System.out.println(str1 + str2);
  }
}
```


-
## Functional Interface
* `BiConsumer` as static method reference

```java
public class MainClass {
  public void demo() {
    BiConsumer<String> consumer = MainClass::someConsumerMethod;
    String result = consumer.accept("Hello", "World");
  }

  public static void someConsumerMethod(String str1, String str2) {
    System.out.println(str1 + str2);
  }
}
```






-
## Functional Interface
* `BiConsumer` as lambda expression

```java
public class MainClass {
  public void demo() {
    BiConsumer<String> consumer
      = (str1, str2) -> System.out.println(str1 + str2);
    consumer.accept("Hello", "World");
  }
}
```






























-
-
## Functional Interface
* `Function` as instance method reference

```java
public class MainClass {
  public void demo() {
    Function<Integer, String> function = this::method;
    String result = function.apply(10);
  }

  public String method(Integer val1) {
    return String.valueOf(val1);
  }
}
```


-
## Functional Interface
* `Function` as static method reference

```java
public class MainClass {
  public void demo() {
    Function<Integer, String> function = MainClass::method;
    String result = function.apply(10);
  }

  public static String method(Integer val1) {
    return String.valueOf(val1);
  }
}
```






-
## Functional Interface
* `Function` as lambda expression

```java
public class MainClass {
  public void demo() {
    Function<Integer, String> function = (intVal) -> String.valueOf(intVal);
    String result = function.apply(10);
  }
}
```






























-
-
## Functional Interface
* `BiFunction` as instance method reference

```java
public class MainClass {
  public void demo() {
    BiFunction<Integer, Long, String> function = this::method;
    String result = function.apply(10);
  }

  public String method(Integer val1, Long val2) {
    return String.valueOf(val1 + val2);
  }
}
```


-
## Functional Interface
* `BiFunction` as static method reference

```java
public class MainClass {
  public void demo() {
    BiFunction<Integer, Long, String> function = MainClass::method;
    String result = function.apply(10);
  }

  public static String method(Integer val1, Long val2) {
    return String.valueOf(val1 + val2);
  }
}
```






-
## Functional Interface
* `BiFunction` as lambda expression

```java
public class MainClass {
  public void demo() {
    BiFunction<Integer, Long, String> function =
      (val1, val2) -> String.valueOf(val1 + val2);
  }
}
```
