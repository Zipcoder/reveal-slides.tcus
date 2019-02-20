# Collections Framework

-
-
# `Iterator` and `Collection` interface


-
## What is a `Collection`?
* `Collection` is an interface which ensures a class has the ability to hold a series of objects.
* Often, we consider `Map` objects to be a `Collection`, although they do not _implement_ the `Collection` interface.



-
-
### Collections and Maps

| List       | Set           | Map
| ---------- | ------------- | --------------- |
| ArrayList  | HashSet       | TreeMap         |
| LinkedList | TreeSet       | HashMap         |
| ArrayDeque | PriorityQueue | WeakHashMap     |
|            | EnumSet       | IdentityHashMap |
|            | LinkedHashSet | LinkedHashMap   |
||||


-
## Collection Interface
* Fundamental interface for `Collection` classes in java.

```java
public interface Collection<E> extends Iterable<E> {
    boolean add(E element);
    boolean addAll(Collection<? extends E> collection);
    void clear();
    boolean contains(Object object);
    boolean containsAll(Collection<?> collection);
    boolean isEmpty();
    Iterator<E> iterator();
    boolean remove(Object object);
    boolean removeAll(Collection<?> collection);
    boolean retainAll(Collection<?> collection);
    int size();
    Object[] toArray();
    <T> T[] toArray(T[] array);
}
```


-
-
## Collection Interface
### `boolean add(E element)`
* Attempts to add an element to the `Collection`
* returns `true` if adding the element changes the `Collection`, else `false`.
* Adding an already-present-object to a `Set` collection will return `false`.

```java
public void demo() {
	Collection<String> set = new HashSet<>();
	String valueToBeAdded = "Hedjet";
	set.add(valueToBeAdded);
	System.out.println(set.add(valueToBeAdded)); // prints false
}
```




-
-
## Collection Interface
### `boolean addAll(Collection)`
* Attempts to add a collection of elements to the `Collection`
* returns `true` if adding the elements changes the `Collection`, else `false`.

```java
public void demo() {
  Collection<String> set = new HashSet<>();
  String[] valuesToBeAdded = {"Froilan", "Wilhem", "Leon", "Nhu", "Kris"};
  Collection<String> valuesAsList = Arrays.asList(valueToBeAdded);
  System.out.println(set.addAll(list)); // prints true
}
```





-
-
## Collection Interface
### `boolean remove(Object)`
* Attempts to remove an object from the `Collection`
* returns `true` if removing the element changes the `Collection`, else returns `false`
* Removing an element that is not present in an `ArrayList` will return `false`.

```java
public void demo() {
  // prints false
  System.out.println(new ArrayList().remove(new Object()));
}
```




-
-
## Collection Interface
### `boolean removeAll(Collection)`
* Attempts to remove a collection of elements from the `Collection`
* returns `true` if removing the elements changes the `Collection`, else returns `false`

```java
public void demo() {
  Collection<String> originalCollection = new ArrayList<>();
  String[] elementsAsArray = {"The", "Quick", "Brown"};
  Collection<String> elementsAsList = Arrays.asList(elementsAsArray);

  // prints false
  System.out.println(originalCollection.removeAll(elementsAsList));
}
```







-
-
## Collection Interface
### `boolean retainAll(Collection)`
* Retains only the elements in this collection that are contained in the specified collection.
* returns `true` if retaining the elements changes the `Collection`, else returns `false`

```java
public void demo() {
  String[] originalArray = {"The", "Quick", "Brown"};
  String[] elementsToBeRetained = {"The", "Quick"};
  List<String> originalList = new ArrayList<>(Arrays.asList(originalArray));
  List<String> retentionList = Arrays.asList(elementsToBeRetained);

  // prints true  
  System.out.println(originalList.retainAll(retentionList)));
}
```










-
-
## Collection Interface
### `boolean isEmpty()`
* returns `true` if the size of the `Collection` is `0`, else returns `false`.

```java
public void demo() {
  String[] elementsAsArray = {"The", "Quick", "Brown"};
  Collection<String> elementsAsList = Arrays.asList(arrayOfStrings);
  System.out.println(elementsAsList.isEmpty()); // prints false
}
```


-
-
## Collection Interface
### `int size()`
* Returns the number of elements in the `Collection`.

```java
public void demo() {
  String[] elementsAsArray = {"The", "Quick", "Brown"};
  Collection<String> elementsAsList = Arrays.asList(arrayOfStrings);
  System.out.println(elementsAsList.size()); // prints 3
}
```




-
-
## Collection Interface
### `void clear()`
* Removes all elements from the `Collection`.

```java
public void demo() {
  String[] elementsAsArray = {"The", "Quick", "Brown"};
  List<String> elementsAsList = new ArrayList<>(Arrays.asList(arrayOfStrings));
  elementsAsList.clear();
  System.out.println(elementsAsList.isEmpty()); // prints true
}
```


-
-
## Collection Interface
### `Object[] toArray()`
* Populates a new `Object[]` with the elements from this `Collection`

```java
public void demo() {
  String[] elementsToAdd = {"The", "Quick", "Brown"};
  List<String> elementList = new ArrayList<>(Arrays.asList(elementsToAdd));
  Object[] listAsArray = elementList.toArray();
}
```




-
-
## Collection Interface
### `Object[] toArray(E[])`
* Populates a new array of the _respective type_ with the elements from this `Collection`

```java
public void demo() {
  String[] elementsToAdd = {"The", "Quick", "Brown"};
  List<String> elementList = new ArrayList<>();
  elementList.foreach((element) -> elementList.add(element));
  int newArrayLength = elementList.size();
  String[] arrayToBePopulated = new String[newArrayLength];
  String[] listAsArray = elementList.toArray(arrayToBePopulated);
}
```











-
-
## Collection Interface<br>`Iterator<E> iterator()`
* Returns an object that implements the `Iterator` interface








-
# Iterable Interface
* `Collection` extends `Iterable`, therefore all `Collection` types are valid candidates for the `foreach` loop.
* All `Iterable`s must provide an implementation for `Iterator<E> iterator()`.
* `Iterable` ensures the implementing class is a valid candidate for the `foreach` loop
* Is **NOT** the same as the `Iterator` interface.

-
-
## Collection Interface
### `void clear()`
* Removes all elements from the `Collection`.

-
-
## Collection Interface
### `int size()`
* Returns the number of elements in the `Collection`.


-
-
## Collection Interface
### `boolean isEmpty()`
* returns `true` if the size of the `Collection` is `0`, else returns `false`.

-
# Iterator interface
* `Iterator` is used to visit the elements in the `Collection`, one by one.

```java
public interface Iterator<E> {
  E next();
  boolean hasNext();
  void remove();
  default void forEachRemaining(Consumer<? super E> action);
}
```



-
-
## Iterator Interface
* Repeatedly calling the `next()` method enables you to visit each element from the collection, one by one.
*  `NoSuchElementException` is thrown upon invoking `next()` on an `Iterator` that has reached the end of the collection.
	* This can be prevented by evaluating the `hasNext()` method before calling `next()`.
	* The compiler translates `foreach` loops into a loop with an iterator.

```java
public static void printIterable(Iterable<Object> iterable) {
    Iterator iterator = iterable.iterator();
    while(iterator.hasNext()) {
        System.out.println("Current Element = " + iterator.next());
    }
}
```





-
-
## Iterator Interface
* As of Java8, you can call the `forEachRemaining` method with a `Consumer` lambda expression.
* The lambda expression is invoked with each element of the iterator, until there are none left.

```java
public static void printIterable(Iterable<Object> iterable) {
  Iterator iterator = iterable.iterator();
  iterator.forEachRemaining((element) -> System.out.println(element));
}
```


-
-
## Iterator Interface<br>`next()`
* Think of Java iterators as being _between_ elements.
* When you call `next`, the iterator jumps over the next element, and it returns a reference to the element that it just passed.
<br><img src = "https://i.giphy.com/media/AkEctVBhHuvtu/giphy-downsized.gif">








-
-
## Iterator Interface<br>`remove()`
* removes the element that was returned by the last call to `next()`
* Often, you may need to view an element before deciding to delete it.
* It is illegal to call `remove()` if it wasn’t preceded by a call to `next()`.

```java
public void deleteFirstElement(Iterator<String> iterator) {
	iterator.next(); // skip first element
	iterator.remove(); // remove first element
}
```


















-
## `AbstractCollection` Class
* The `Collection` interface declares 18 methods.
* To avoid implementing a lot of the fundamental methods, the `Collection` library developers created an `AbstractCollection` class.
* AbstractCollection has a concrete implementation of all `Collection` methods except `size()` and `iterator()`




-
-
### Sample `AbstractCollection` implementation

```java
public class MyCollection<E> extends AbstractCollection<E>  {
    private final Iterable<E> iterable;
    public MyCollection(Iterable<E> iterable) {
    	this.iterable = iterable;
    }

    @Override
    public Iterator<E> iterator() {  return iterable.iterator(); }

    @Override
    public int size() {
        List<E> list = new ArrayList<>();
        iterator().forEachRemaining(list::add);
        return list.size();
    }
}
```










-
# Puppies
