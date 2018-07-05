# Collections Framework
## Part 2


-
# Collection Interface
* Fundamental interface for collection classes in java
* Below is a simplified version for the sake of brevity

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
*  `NoSuchElementException` is thrown upon invoking `next()` on an `Iterator` that has reached the of the collection.
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
# Iterator Interface
* As of Java8, you can call the `forEachRemaining` method with a `Consumer` lambda expression.
* The lambda expression is invoked with each element of the iterator, until there are none left.```
public static void printIterable(Iterable<Object> iterable) {
    Iterator iterator = iterable.iterator();
	iterator.forEachRemaining((element) -> System.out.println(element));
}
```


-
-
# Iterator Interface<br>`next()`
* Think of Java iterators as being _between_ elements.
* When you call `next`, the iterator jumps over the next element, and it returns a reference to the element that it just passed.
<br><img src = "https://i.giphy.com/media/AkEctVBhHuvtu/giphy-downsized.gif">








-
-
# Iterator Interface<br>`remove()`
* removes the element that was returned by the last call to next
* Often, you may need to view an element before deciding to delete it.
* It is illegal to call `remove()` if it wasn’t preceded by a call to `next()`.

```java
public void deleteFirstElement(Iterator<String> iterator) {
	iterator.next(); // skip first element
	iterator.remove(); // remove first element
}
```













-
# Queue Interface
* Specifies that you can
	* add elements at the tail end of the queue,
	* remove elements at the head,
	* find out how many elements are in the queue. 

	
-
-
### Minimal Form of a Queue Interface
* The interface tells you nothing about how the queue is implemented.

```java
// a simplified form of the interface in the standard library
public interface Queue<E> {	void add(E element);
	E remove();
	E peek();	int size();}
```



-
-
### Queue API Structure
* There are 3 primary types of Queue-Operations
	* Adding: `add(e)` /  `offer(e)`
	* Removing: `remove()` / `poll()`
	* Viewing: `element()` / `peek()`




-
## Queue API Stucture: Adding


-
-
### Queue API Stucture<br>`add(e)`
* Adds an element to the tail of the Queue.
* Has potential to throw `IllegalStateException`
	* `IllegalStateException` if the element cannot be added at this time due to capacity restrictions
	* `ClassCastException` if the class of the specified element prevents it from being added to this queue
	* `NullPointerException` if the specified element is null and this queue does not permit null elements
	* `IllegalArgumentException` if some property of this element prevents it from being added to this queue


-
-
### Queue API Structure<br>`offer(e)`
* Adds an element to the tail of the Queue.
* Does not have potential to throw `IllegalStateException`
	* `ClassCastException` if the class of the specified element prevents it from being added to this queue
	* `NullPointerException` if the specified element is null and this queue does not permit null elements
	* `IllegalArgumentException` if some property of this element prevents it from being added to this queue






-
## Queue API Stucture: Removing

-
-
### Queue API Strucutre<br>`remove()`
* Removes element at the head of the Queue.
* Has potential to throw an exception.
	* `NoSuchElementException` if this queue is empty

-
-
### Queue API Structure<br>`poll()`
* Removes element at the head of the Queue.
* Throws no exceptions




-
## Queue API Stucture: Viewing



-
-
### Queue API Structure<br>`element()`
* Retrieves, but does not remove, the head of the queue.
* Has the potential to throw an exception
	* `NoSuchElementException` if this queue is empty



-
-
### Queue API Structure<br>`peek()`
* Retrieves, but does not remove, the head of the queue.
* Throws no exceptions





### All About Queues
* Read [here](http://www.codejava.net/java-core/collections/java-queue-collection-tutorial-and-examples)







