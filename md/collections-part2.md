# Collections Framework
## Part 2


-
# Collection Interface
* Fundamental interface for collection classes in java

```java
public interface Collection<E> implements Iterable<E> {	boolean add(E element);
	boolean remove(E element);
	int size();
	Iterator<E> iterator();}
```


-
-
## Collection Interface<br>`boolean add(E element)`
* Attempts to add an element to the collection
* returns `true` if adding the element changes the collection
* returns `false` if the collection is unchanged after the addition
* Adding an already-present-object to a `Set` collection will return `false`

```
// prints `false`
Set<String> set = new HashSet<String>(stringArrayList);
System.out.println(set.add("The"));
```



-
-
## Collection Interface<br>`boolean remove(E e)`
* Attemps to remove an element from the collection
* returns `true` if removing the element changes the collection
* returns `false` if the collection is unchanged after removal
* Removing an element that is not present in an `ArrayList` will return `false`.

```java
// prints `false`
System.out.println(new ArrayList<>().remove(new Object()));
```


-
-
## Collection Interface<br>`Iterator<E> iterator()`
* Returns an object that implements the `Iterator` interface













-
# Iterator Interface
* `Collection` extends `Iterator`, therefore all `Collection` types are valid candidates for the `foreach` loop
* Is used to visit the elements in the collection one by one.

```java
public interface Iterator<E> {	E next();	boolean hasNext();	void remove();	default void forEachRemaining(Consumer<? super E> action);}
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
* The lambda expression is invoked with each element of the iterator, until there are none left.```
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
