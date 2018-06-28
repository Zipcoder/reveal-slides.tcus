


-
-
#Streams
1. What is
2. Usage
3. Section 8.2 - Stream Creation
4. Section 8.4 - Extracting Substreams
5. Section 8.4 - Combining Streams
4. Section 8.3 - Stream Transformations: `filter`, `map`, `flatMap`
5. Section 8.5 - More Stream Transformations: `distinct`, `sorted`
6. Section 8.6 - Reductions (Terminal Operations) `count`, `max`, `min`, ... , `andMore`





-
-
#What is a Stream?

* Basic generalization of lists; a sequence of objects
* Specify what to be done, rather than how to do it
* Instead of operating on elements of a list, you apply the functions to the stream itself
* Can operate on elements in parallel



-
-
#Usage
```Java
// This method definition showcases for-each-loop syntax
public void forloopPrint(String[] stringArray) {
    for (String currentString : stringArray) {
        System.out.println(currentString);
    }
}

// This method definition showcases Stream.forEach syntax
public void streamPrint(String[] stringArray) {
    Stream<String> stringStream = Stream.of(stringArray);
    stringStream.forEach(System.out::print);
}
```









-
-
#*Section 8.2*<br>Stream Creation

-
#From Array, strategy 1
```Java
/** @param stringArray source array to create stream
 *  @return stream representation of this array */
public Stream<String> fromArray1(String[] stringArray) {
    Stream<String> stringStream = Arrays.stream(stringArray);
    return stringStream;
}
```

-
#From Array, strategy 2
```Java
/** @param stringArray source array to create stream
 *  @return stream representation of this array */
public Stream<String> fromArray2(String[] stringArray) {
    Stream<String> stringStream = Stream.of(stringArray);
    return stringStream;
}
```
*Two ways of getting the same result.*
*And one should not be surprised to find this from time to time
in the Java Standard Libraries.*

-
#From varargs
```Java
/** @return stream representation of this array */
public Stream<String> fromVarargs() {
    Stream<String> stringStream = Stream.of("The", "Quick", "Brown", "Fox", "Jumps");
    return stringStream;
}
```

-
#From List
```Java
/** @param stringList source list to create stream
*   @return stream representation of this List */
public Stream<String> fromList(List<String> stringList) {
    Stream<String> stringStream = stringList.stream();
    return stringStream;
}
```


-
#`.generate`, strategy 1
```Java
/** @return endless stream */
public Stream<String> fromGenerator1() {
    Stream<String> stringStream = Stream.generate(() -> "Hello World");
    return stringStream;
}
```

-
#`.generate`, strategy 2
```Java
/** @return endless stream */
public Stream<Double> fromGenerator2() {
    Stream<Double> randoms = Stream.generate(Math::random);
    return randoms;
}
```

-
#`.empty`
```Java
public Stream<String> fromEmpty() {
	return Stream.empty();
}
```

#`.iterate`
```Java
public Stream<String> fromIterator() {
	return Stream.iterate(BigInteger.ZERO, n -> n.add(BigInteger.ONE));
}
```












-
-
#*Section 8.4*<br>Extracting substreams

-
#Extracting substreams
```Java
public Stream<String> getSubStream(String[] stringArray, int startIndex, int endIndex) {
	return Arrays.stream(stringArray, startIndex, endIndex);
}

public Stream<String> getSubStream(String[] stringArray, int endIndex) {
	return Arrays.stream(stringArray).limit(endIndex);
}
```










-
#*Section 8.4*<br>Combining substreams

-
#`Stream.concat`
* `Stream.concat` concatenates two streams

```Java
public Stream<String> combineStreams(String[] array1, String[] array2) {
    Stream<String> stream1 = Arrays.stream(array1);
    Stream<String> stream2 = Arrays.stream(array2);

    return Stream.concat(stream1, stream2);
}
```















-
-
#*Section 8.3, 8.5*<br>Transformations

-
#Filter, Map, and FlatMap
* The `filter` transformation yields a new stream with elements that match the specified criteria
* The `map` transformation takes an argument of a function, applies the function to each element, and returns the respective stream
* The `flatMap` transformation prevents nested stream structures like `Stream<Stream<String>>`


-
#Filter
* The `filter` transformation yields a new stream with elements that match the specified criteria

```Java
public Stream<String> getStringsShorterThan(String[] stringArray, int length) {
    return Arrays.stream(stringArray)
            .filter(word -> word.length() > length);
}

public Stream<String> getStringsLongerThan(String[] stringArray, int length) {
    return Arrays.stream(stringArray)
            .filter(word -> word.length() < length);
}
```


-
#Map
* The `map` transformation takes an argument of a function, applies the function to each element, and returns the respective stream

```Java
public Stream<String> letters(String someWord) {
    String[] characters = someWord.split("");
    return Stream.of(characters);
}

public Stream<Stream<String>> wordsMap(String[] someWords) {
    return Stream.of(someWords).map(w -> letters(w));
}
```

-
#FlatMap
* The `flatMap` transformation prevents nested stream structures like `Stream<Stream<String>>`

```Java
public Stream<String> letters(String someWord) {
    String[] characters = someWord.split("");
    return Stream.of(characters);
}

public Stream<String> wordsFlatMap(String[] stringArray) {
    Stream<String> wordStream = Stream.of(stringArray);
    List<String> wordList = wordStream.collect(Collectors.toList());
    return wordList.stream().flatMap(w -> letters(w));
}
```


-
#Distinct
* The `distinct` transformation returns the stream with duplicates removed

```Java
public Stream<String> uniqueWords(String... words) {
	return Stream.of(words).distinct();
}
```


-
#Transformations: Sorted
```Java
public Stream<String> sort(String[] words) {
    return Arrays.stream(words).sorted();
}
```














-
-
#*Section 8.6*<br>Simple Reductions
* Reductions are terminal operations
* They reduce the stream to a nonstream value that can be used in the program.
* Examples include: `.count`, `.max`, `.min`, `.findFirst`, `.findAny`, `.anyMatch`
* Reductions yield `Optional<T>` values


-
#`Optional<T>`
* An `Optional<T>` value either wraps the result of a method, or indicates that there is none
* The purpose of `Optional<T>` is to prevent potential `NullPointerExceptions`


-
#`.count`
```Java
/** @return number of elements in an array using a stream */
public int getCount(String[] stringArray) {
    return (int) Arrays.stream(stringArray).count();
}
```


-
#`.min`, `.max`
```Java
/** @return longest String object in an array using a stream */
public Optional<String> getMax(String[] stringArray) {
    return Arrays.stream(stringArray).max(String::compareToIgnoreCase);
}

/** @return longest String object in an array using a stream */
public Optional<String> getMax(String[] stringArray) {
    return Arrays.stream(stringArray).min(String::compareToIgnoreCase);
}
```



-
#`.findFirst`, `.findAny`
```Java
/** @return get first String from an array using a stream */
public Optional<String> getFirst(String[] stringArray) {
    return Arrays.stream(stringArray).findFirst();
}

/** @return a random string in an array using a stream */
public Optional<String> getRandom(String[] stringArray) {
    return Arrays.stream(stringArray).findAny();
}
```

















-
-
#END OF LECTURE























-
-
#More Streams...
1. Optional Type
2. Optional Type Usage
3. Optional Type Misuse















-
-
#*Section 8.7.3*<br>Optional Type Creation




-
# `.of`, `.empty`
```Java
public static Optional<Double> inverse(Double x) {	return x == 0 ? Optional.empty() : Optional.of(1 / x);}
```




-
# `.ofNullable`
* The `ofNullable` method is intended as a bridge from possibly null values to optional values.

```Java
/** return Optional.of(obj) if obj is not null, else
  * return Optional.empty() otherwise. */
public static Optional<String> demoOfNullable(String arg) {
	return Optional.ofNullable(arg);
}
```



















-
-
#*Section 8.7.4*<br>Composing Optional Value Functions<br>With `.flatMap`



-
#Chaining Method Calls
* Consider the following:
	* `S` is a class which contains the definition of method `.f()`
	* Method `.f()` returns `Optional<T>`
	* `T` is a class which contains the definition of method `.g()`
	* Method `.g()` returns `Optional<U>`
	* you would be able to call them, via <br>`s.f().g()`<br>if these methods returned the raw types rather than the respective `Optional` wrapper type.

-
#Avoiding `Optional` usage puts code<br>at risk of `NullPointerException`
```
S s = new S();
U u = s.f().g();
```


-
#`.map` yields (potentially) nested structure
```
S s = new S();
Optional<Optional<U>> = s.f().map(T::g);
```


-
#`.flatMap` yields flat structure
```
S s = new S();
Optional<U> = s.f().flatMap(T::g);
```















-
-
#*Section 8.8*<br>Collecting Results

-
#Method References `::`
* Because Java 7 has no syntax to enable a method being passed as an argument, the `::` syntax was introduced in Java 8 to reference methods.

```
Consumer<String> stringConsumer = System.out::print;
List<String> stringList = ...
Stream<String> s = stringList.stream();
s.forEach(stringConsumer);
```


-
#More Method References `::`
```
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


-
#Using `.toArray()` to collect results
* It is not possible to create a generic array at runtime.<br>If you want an array of the correct type, pass in the array constructor.

```
public class CollectorsDemo {
    private final List<String> list;
    public CollectorsDemo(List<String> list) {
        this.list = list;
    }

    private Stream<String> toStream() {
        return list.stream();
    }

    public String[] toArray() {
    	return toStream().toArray(String[]::new);
    }
}
```




-
#Using `.collect()`<br>to collect to a `List`

```
public class CollectorsDemo {
    private final List<String> list;
    public CollectorsDemo(List<String> list) {
        this.list = list;
    }

    private Stream<String> toStream() {
        return list.stream();
    }

    public List<String> toList() {
        return toStream().collect(Collectors.toList());
    }
}
```



-
#Using `.collect()`<br>to collect to a `Set`

```
public class CollectorsDemo {
    private final List<String> list;
    public CollectorsDemo(List<String> list) {
        this.list = list;
    }

    private Stream<String> toStream() {
        return list.stream();
    }

    public Set<String> toSet() {
        return stringStream.collect(Collectors.toSet());
    }
}
```
















-
-
#*Section 8.9*<br>Collecting into Maps

-
#Using `.collect()`<br>to collect to a `Map`

```
public class CollectorsDemo {
    private final List<String> list;
    public CollectorsDemo(List<String> list) {
        this.list = list;
    }

    private Stream<String> toStream() {
        return list.stream();
    }

    public Map<Integer, String> toMap() {
        return stringStream.collect(
        	Collectors.toMap(String::hashCode, String::toString));
    }
}
```

-
#Using `.collect()`<br>to collect to a `Map`
* In the common case, when the values should be the actual elements, use `Function.identity()` instead.

```Java
public class CollectorsDemo {
    private final List<String> list;
    public CollectorsDemo(List<String> list) {
        this.list = list;
    }

    private Stream<String> toStream() {
        return list.stream();
    }

    public Map<Integer, String> toMap() {
        return stringStream.collect(
        	Collectors.toMap(String::hashCode, Function::identity));
    }
}
```












-
-
#*Section 8.10*<br>Grouping and Partitioning


-
#`.groupingBy()`
* Forming groups of values with the same characteristic is very common, and the `groupingBy` method supports it directly.

```Java
public Map<String, List<Locale>> groupingByDemo() {
    Stream<Locale> locales = LocaleFactory.createLocaleStream(999);
    return locales.collect(Collectors.groupingBy(Locale::getCountry));
}
```

-
#`.partitioningBy()`
* Partitioning is a another grouping approach, in which the resultant Map contains two different groups, one for true values and another for false values.



-
#Relevant Functional Interfaces
* A `Function` is a single-argument, non-void-returning operation.
* A `Predicate` is a single-argument, boolean-returning operation.
* A `Consumer` is a single-argument, void-returning operation.
* A `classifier` is a predicate used to group a stream.
* A `lambda` is a function which can be created without belonging to any class.
* A `method reference` is how java handles the nuance of passing methods as arguments.


























-
#*Section 8.11*<br>Downstream Collectors
* The `.groupingBy` method yields a map whose values are lists
* If you want to process those lists in some way, supply a _downstream collector_.
* For example, if you want sets, instead of lists, you can use `Collectors.toSet`

```Java
class Demo {
	public Map<String, Set<Locale>> demoDownstreamCollectors1() {
	    Stream<Locale> locales = LocaleFactory.createLocaleStream();
	    Map<String, Set<Locale>> countryToLocaleSet = locales.collect(
	            groupingBy(Locale::getCountry, toSet()));

	    return countryToLocaleSet;
	}
}
```

-
#Downstream Collectors
* Several collectors are provided for reducing grouped elements to numbers.
* `counting` produces a count of collected elements.

```Java
class Demo {
    public Map<String, Long> demoDownstreamCollectors2() {
        Stream<Locale> locales = LocaleFactory.createLocaleStream();
        Map<String, Long> countryToLocaleSet = locales.collect(
                groupingBy(Locale::getCountry, counting()));

        return countryToLocaleSet;
    }
}
```















-
-
#*Section 8.12*<br>Reduction Operations
* The `reduce` method is a general mechanism for computing a value from a stream.

```Java
class Demo {
IntegerFactory integerFactory = new IntegerFactory(0, 999);
    public void demo() {
        List<Integer> values = integerFactory.createList(100);
        Optional<Integer> sum = values.stream().reduce((x, y) -> x + y);
    }
}
```
If `•` is some reduction operation.<br>
Then, the reduction yield is<br>
v<sub>0</sub> • v<sub>1</sub> • v<sub>2</sub> ..., <br>
where v<sub>i</sub> • v<sub>i+1</sub> <br>
represents the function call •(v<sub>i</sub>, v<sub>i+1</sub>)






















-
-
#*Section 8.13*<br>Primitive Type Streams
* The stream library has specialized types `IntStream`, `LongStream`, and `DoubleStream` that store primitive values directly without using wrappers.
* If you want to store `int`, `short`, `char`, `byte`, and `boolean`, use an `IntStream`.
* If you want to store `float`, or `double`, use `DoubleStream`.

-
#Populate `IntStream`
* Use `.of` to populate a stream with respective values

```Java
class PrimitiveStreams {
    public IntStream demoOf() {
        IntStream intStream = IntStream.of(1, 3, 5, 8, 13);
        return intStream;
    }
}
```


-
#Generate `IntStream`
* Use `.generate` to create endless stream

```Java
class PrimitiveStreams {
    public IntStream demoGenerate() {
        IntegerFactory integerFactory = new IntegerFactory(0,999);
        IntStream intStream = IntStream.generate(integerFactory::createInteger);
        return intStream;
    }
}
```


-
#Create exclusive range `IntStream`
* Use `.range` to generate a set of ints between specified `min` and `max` values

```Java
class PrimitiveStreams {
    /** upper bound is excluded
     *  @param min value to generate
     *  @param max value to generate
     *  @return range of numbers betwen min and max */
    public IntStream demoRange(int min, int max) {
        return IntStream.range(min, max);
    }
}
```


-
#Create inclusive range `IntStream`
* Use `.range` to generate a set of ints between specified `min` and `max` values

```Java
class PrimitiveStreams {
    /** upper bound is included
     *  @param min value to generate
     *  @param max value to generate
     *  @return range of numbers betwen min and max */
    public IntStream demoRange(int min, int max) {
        return IntStream.range(min, max);
    }
}
```

-
#Converting from Primitive to Object stream
* Use the `.boxed` method to convert form primitive streams to object streams

```Java
public class PrimitiveStreams {
    public Stream<Integer> demoBoxStream() {
        return IntStream.of(0, 1, 2).boxed();
    }
}
```
















-
-
#*Section 8.14*<br>Parallel Streams
* Streams make it easy to parallelize bulk operations.
* The process is mostly automatic, but you need to take note of the following:
	1. Use a parallel stream
		* You can get a parallel stream via `Collection.parallelStream()`
		* You can convert to parallel stream via `Stream.of(wordArray).parallel()`
	2. 	If a stream is in parallel mode when the terminal method executes, all intermediate stream operations will be parallelized.
	3. Operations are stateless and can be executed in an arbitary order.



-
#Improper usage
* Here is an example of somethign you cannot do.
* Suppose you want to count all short words in a stream of strings.

```Java
class Demo {
	public void demo() {
		int[] shortWrods = new int[12];
		words.parallelStream().forEach(
			s -> { if(s.length <12) shorts[s.length()]++; });
			// Error - race condition!
		System.out.println(Arrays.toString(shortWords));
	}
}
```
* The function passed to `forEach` runs concurrently in multiple threads, each updating a shared array.


-
#Proper usage
* It is your responsibility to ensure that any functions passed to parallel stream operations are safe to execute in parallel.
* The best way to do that is to stay away from mutable state.
* In this example, you can safely parallelize the computation if you group strings by length and count them.

```Java
class Demo {
	public Map<Integer, Long> wordCountMap(Stream<String> words) {
		Map<Integer, Long> shortWordCounts = words.paralellStream()
			.filter(s -> s.length() < 10)
			.collect(groupingBy(String::length, counting()));
	}
}
```
