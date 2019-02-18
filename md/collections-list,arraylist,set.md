# Lists, ArrayLists, Sets

-
## What is a `List`?
* An ordered collection of elements.
* Users of `List` objects can:
  * access elements by their integer index (position in `List`)
  * insert elements into `List`
  * remove elements from the `List` by their integer index
  * search for the presence of element in the `List` by their integer index


-
## List Interface
```java
public interface List<E> extends Collection<E> {
  int lastIndexOf(Object object);
  int indexOf(Object object);
  E set(int index, E object);
  List<E> sub(int startingIndex, int endingIndex);
}
```

-
## ArrayList
* Dynamic in capacity (size)
  * size increases upon being populated, decreases upon removal
* Quicker than `LinkedList` with random access to elements.
* Can only handle _non-primitive_ types.




-
-
## `Set` Interface
* Very similar to `List` interface, except
	* the `add` method should reject duplicates.
	* the `equals` method should be defined so that two sets are identical if they have the same elements, but not necessarily in the same order.
	* the `hashCode` method should be defined so that two sets with the same elements yield the same hash code.


-
-
### Sets
||||
| --------------- | ------------ |
| `HashSet`         | sorts elements in hash order (fast, not predictable)
| `TreeSet`         | sorts elements in ascending sorted order
| `LinkedHashSet`   | provides insertion-order access to elements
| `EnumSet`         | Optimized set for holding `enum` instances
| `BitSet`          | Optimized for storing sequences of bits
||||




-
-
## `HashSet`
* Does not maintain insertion-order of elements
* Elements are positioned by their _hash_ value.
* Can retrieve elements quickly due to efficient hash-sorting



-
### `HashSet` example
```java
public void test() {
    String[] words = {"John", "Charles", "Cutler", "Tuskegee"};
    Set<String> set = new HashSet<>(Arrays.asList(words));
    System.out.println(set);
}
```

Output

```
[Charles, Tuskegee, John, Cutler]
```




-
-
## `TreeSet`
* Similar to `HashSet`
* Sorts elements after each insertion
  * Sort is dependent on implementation of `compareTo` or `toString` if elements not `Comparable`









-
### `TreeSet` example

```java
public void test() {
    String[] words = {"John", "Charles", "Cutler", "Tuskegee"};
    Set<String> set = new TreeSet<>(Arrays.asList(words));
    System.out.println(set);
}
```

Output
```
[Charles, Cutler, John, Tuskegee]
```







-
-
## `LinkedHashSet`
* provides insertion-order access to element


-
### `LinkedHashSet` example

```java
public void test() {
    String[] words = {"John", "Charles", "Cutler", "Tuskegee"};
    Set<String> set = new TreeSet<>(Arrays.asList(words));
    System.out.println(set);
}
```

Output
```
[John, Charles, Cutler, Tuskegee]
```
