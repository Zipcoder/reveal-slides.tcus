# Lists, ArrayLists, LinkedLists, sets

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
-
## List types
* `ArrayList`
* `LinkedList`


-
## ArrayList
* Quicker than `LinkedList` with access to random (arbitrary) elements.


-
-
## LinkedList
* Values are stored as `Node` objects
* Each `Node` is a separate object with a `data` and `address` field.
* Quicker than `ArrayList` at removal/insertion of elements in the middle of the list.


-
### LinkedList

```java
class LinkedList {
  Node head;

  class Node {
    int data;
    Node next;

    Node(int d) {
      data = d;
      next = null;
    }
  }

}
```




-
-
## `Set` Interface
* Very similar to `List` interface, except
	* the `add` method should reject duplicates.
	* the `equals` method should be defined so that two sets are identical if they have the same elements, but not necessarily in the same order.
	* the `hashCode` method should be defined so that two sets with the same elements yield the same hash code.


-
### `Set` types
* `HashSet` - keeps elements in hash order (fast, not predictable)
* `TreeSet` - keeps elements in ascending sorted order
* `LinkedHashSet` - provides insertion-order access to elements
* `BitSet` - Optimized for storing sequences of bits
* `EnumSet` - Optimized set for holding `enum` instances
* Sets are often used to test for membership using the `contains()` method



-
-
## `HashSet`
* Does not maintain order of elements
* Positions elements by their _hash_ value.
* Can retrieve elements quickly due to efficient hash-sorting
* If you implement your own `HashSet` you are responsible for overriding the `hashCode` method to behave as you expect



-
### `HashSet` example
```java
public void test() {
    String[] words = {"John", "Charles", "Cutler", "Tuskegee"};
    Set<String> set = new HashSet<>(Arrays.asList(words));
    System.out.println(Arrays.toString(set.toArray()));
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
* Can only be populated by objects which implement `Comparable`
* Sorts elements after each insertion
  * Sort is dependent on implementation of `compareTo`


-
### `TreeSet` example

```java
public void test() {
    String[] words = {"John", "Charles", "Cutler", "Tuskegee"};
    Set<String> set = new TreeSet<>(Arrays.asList(words));
    System.out.println(Arrays.toString(set.toArray()));
}
```

Output
```
[Charles, Cutler, John, Tuskegee]
```
