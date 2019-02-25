# Lambda Expressions


-
-
## Deducing java Lambda Expressions


-
### Function as BiFunction
* Given a function `Function<Integer, String>`, one can express a behaviorally equivalent `BiFunction<Integer, Object, String>`.

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
* Given a function `Function<Integer, String>`, one can express a behaviorally equivalent `BiFunction<Integer, Object, String>`.

```java
public void demo() {
  Function<Integer, String> function
    = (intVal) -> intVal.toString();

  BiFunction<Integer, Object, String> biFunction
    = (intVal, obj) -> function.apply(intVal);
}
```










-
### BiConsumer as BiFunction
* Given a biconsumer `BiConsumer<Integer, Object>`, one can express a behaviorally equivalent `BiFunction<Integer, Object, Object>`.

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
* Given a function `Function<Integer, String>`, one can express a behaviorally equivalent `BiFunction<Integer, Object, String>`.

```java
public void demo() {
  Function<Integer, String> function
    = (intVal) -> intVal.toString();

  BiFunction<Integer, Object, String> biFunction
    = (intVal, obj) -> function.apply(intVal);
}
```














-
# Consumer as Function
* Given a consumer `Consumer<String>`, one can express a behaviorally equivalent `BiFunction<String, Object, Integer>`.

```java
public void demo() {
  Function<String, Integer> function = (string) -> string.length();
  BiFunction<String, Object, Integer> biFunction = (string, obj) -> function.apply(string);
}
```
