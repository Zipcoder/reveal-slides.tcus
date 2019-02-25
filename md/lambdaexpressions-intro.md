# Lambda Expressions








-
-
## Functional Interfaces


-
## What is a Functional Interface?
* Functional Interface is a way for java8 to handle lambda expressions
* An annotation



-
# Relevant Functional Interfaces
| Name       | Returns | Takes Argument 1 | Takes Argument 2 |
|------------|---------|------------------|------------------|
| Runnable  | No      | No       | No      |
| Supplier   | Yes     | No       | No      |
| Consumer   | No      | Yes      | No      |
| BiConsumer | No      | Yes      | Yes     |
| Function   | Yes     | Yes      | No      |
| BiFunction | Yes     | Yes      | Yes     |


-
# Relevant Functional Interfaces
* A `Runnable` is a no-argument, void-returning operation.
* A `Function` is a single-argument, non-void-returning operation.
* A `Predicate` is a single-argument, boolean-returning operation.
* A `Consumer` is a single-argument, void-returning operation.
* A `Supplier` is a no-argument, non-void-returning operation.
* A `BiConsumer` is a two-argument, void-returning operation.
* A `BiFunction` is a two-argument, non-void-returning operation.


-
## Lambda Expressions
### Runnable
* 

-
## What is a Lambda Expression?



-
# Relevant Jargon
* A `classifier` is a predicate used to group a stream.
* A `lambda` is a function which can be created without belonging to any class.
* A `method reference` is how java handles the nuance of passing methods as arguments.



















-
#Method References `::`
* Because Java 7 has no syntax to enable a method being passed as an argument, the `::` syntax was introduced in Java 8 to reference methods.

```java
Consumer<String> stringConsumer = System.out::print;
List<String> stringList = ...
Stream<String> s = stringList.stream();
s.forEach(stringConsumer);
```


-
#More Method References `::`
```java
class SquareMaker {
     public double square(double num){
        return Math.pow(num , 2);
    }
}

class DemoSquareMaker {
	public static void main(String[] args) {
		SquareMaker squareMaker = new SquareMaker();
		Function<Double, Double> squareMethod = squareMaker::square;
		double ans = squareMethod.apply(23.0);
	}

}
```
