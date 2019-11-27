# Lambda Expressions: Reducing Java Lambda Expressions






-
-

## What we'll cover:
<ul>
<li class="fragment fade-up"> How do I express a more complex lambda as a less complex lambda?
<ul>
<li class="fragment fade-up">Function as BiFunction</li>
<li class="fragment fade-up">BiConsumer as BiFunction</li>
<li class="fragment fade-up">Consumer as BiConsumer</li>
<li class="fragment fade-up">Predicate as Function</li>
<li class="fragment fade-up">Supplier as Function</li>
<li class="fragment fade-up">Runnable as Supplier</li>
</ul></li></ul>








-
### Functional Interfaces
| Name       | Has Return-Value | Takes Argument 1 | Takes Argument 2 |
|------------|---------|------------------|------------------|
| Runnable  | No      | No       | No      |
| Supplier   | Yes     | No       | No      |
| Consumer   | No      | Yes      | No      |
| BiConsumer | No      | Yes      | Yes     |
| Function   | Yes     | Yes      | No      |
| BiFunction | Yes     | Yes      | Yes     |


-
### Functional Interfaces
* A `Runnable` is no-argument, void-returning.
* A `Function` is single-argument, non-void-returning.
* A `Predicate` is single-argument, boolean-returning.
* A `Consumer` is single-argument, void-returning.
* A `Supplier` is no-argument, non-void-returning.
* A `BiConsumer` is two-argument, void-returning.
* A `BiFunction` is two-argument, non-void-returning.


-
### Function as BiFunction
* Given a function `Function<Integer, String>`, one can express a functionally equivalent `BiFunction<Integer, Object, String>`.
  * By omitting the second argument of the `BiFunction`, we acquire a single-argument method

```java
public void demo() {
  Function<Integer, String> function
    = (intVal) -> intVal.toString();

  BiFunction<Integer, Object, String> biFunction
    = (intVal, obj) -> intVal.toString();
}
```



-
### Function as BiFunction
* Given a function `Function<Integer, String>`, one can express a functionally equivalent `BiFunction<Integer, Object, String>`.
  * By encapsulating the `function` call in the `biFunction` definition, we acquire identical behavior

```java
public void demo() {
  Function<Integer, String> function
    = (intVal) -> intVal.toString();

  BiFunction<Integer, Object, String> biFunction
    = (intVal, obj) -> function.apply(intVal);
}
```









-
-
### BiConsumer as BiFunction
* Given a biconsumer `BiConsumer<Integer, String>`, one can express a functionally equivalent `BiFunction<Integer, Object, String>`.
  * By ignoring the return-value of the `biFunction`, we acquire a two-argument void-returning operation

```java
public void demo() {
    BiConsumer<Integer, String> consumer = (intVal, stringVal) ->
            System.out.println(intVal + stringVal);

    BiFunction<Integer, String, Object> biFunction = (intVal, stringVal) -> {
        System.out.println(intVal + stringVal);
        return null;
    };
}
```




-
### BiConsumer as BiFunction
* Given a biconsumer `BiConsumer<Integer, String>`, one can express a functionally equivalent `BiFunction<Integer, Object, String>`.

```java
public void demo() {
    BiConsumer<Integer, String> consumer = (intVal, stringVal) ->
            System.out.println(intVal + stringVal);

    BiFunction<Integer, String, Object> biFunction = (intVal, stringVal) -> {
        consumer.accept(intVal, stringVal);
        return null;
    };
}
```




















-
-
### Consumer as BiConsumer
* Given a consumer `Consumer<String>`, one can express a functionally equivalent `BiConsumer<String, Object>`

```java
public void demo() {
  Consumer<String> consumer = (name) -> System.out.println("Hello " + name);
  BiConsumer<String, Object> = (name, obj) -> System.out.println("Hello " + name);
}
```






-
### Consumer as BiConsumer
* Given a consumer `Consumer<String>`, one can express a functionally equivalent `BiConsumer<String, Object>`

```java
public void demo() {
  Consumer<String> consumer = (name) -> System.out.println("Hello " + name);
  BiConsumer<String, Object> = (name, obj) -> consumer.accept(name);
}
```

















-
-
### Predicate as Function
* Given a predicate `Predicate<String>` one can express a functionally equivalent `Function<String, Boolean>`

```java
public void demo() {
    Predicate<String> predicate = (string) ->
        string.startsWith("A");

    Function<String, Boolean> function = (string) ->
        string.startsWith("A");
}
```





-
### Predicate as Function
* Given a predicate `Predicate<String>` one can express a functionally equivalent `Function<String, Boolean>`

```java
public void demo() {
    Predicate<String> predicate = (string) ->
        string.startsWith("A");

    Function<String, Boolean> function = (string) ->
        predicate.test(string);
}
```
















-
-
### Supplier as Function
* Given a supplier, `Supplier<String>` one can express a functionally equivalent `Supplier<Object, String>`

```java
public void demo() {
    Supplier<String> supplier = () ->
        String.valueOf(System.currentTimeMillis());

    Function<String, Boolean> function = (obj) ->
        String.valueOf(System.currentTimeMillis());
}
```




-
### Supplier as Function
* Given a supplier, `Supplier<String>` one can express a functionally equivalent `Function<Object, String>`

```java
public void demo() {
    Supplier<String> supplier = () ->
        String.valueOf(System.currentTimeMillis());

    Function<String, Boolean> function = (obj) ->
        supplier.get();
}
```




-
-
### Runnable as Supplier
* Given a runnable, `Runnable` one can express a functionally equivalent `Supplier<Object>`
```java
public void demo() {
    Runnable runnable = () ->
        System.out.println("Hello world!");

    Supplier<Object> supplier = () -> {
      System.out.println("Hello world!");
      return null;
    };
}
```
