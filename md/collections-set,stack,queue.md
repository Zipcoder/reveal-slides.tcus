# Collections Framework


-
## `Set` Interface
* Identical to `Collection` interface, except
	* the `add` method should reject duplicates.
	* the `equals` method should be defined so that two sets are identical if they have the same elements, but not necessarily in the same order.
	* the `hashCode` method should be defined so that two sets with the same elements yield the same hash code.





-
-
## `HashSet`
* Does not maintain order of elements
* Positions elements by their _hash_ value.
* Can retrieve elements quickly due to efficient hash-sorting
* If you implement your own `HashSet` you are responsible for overriding the `hashCode` method to behave as you expect



-
-
## `TreeSet`
* Similar to `HashSet`
* Maintains order of elements.
* Can only handle objects which implement `Comparable`




-
# Queue Interface
* Specifies that you can
	* add elements at the tail end of the queue,
	* remove elements at the head,
	* find out how many elements are in the queue.


-
-
### Minimal Form of a Queue Interface
* The interface describes the intent without detailing the implementation

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
## Queue API Structure: Adding


-
-
### Queue API Structure<br>`add(e)`
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
## Queue API Structure: Removing

-
-
### Queue API Structure<br>`remove()`
* Removes element at the head of the Queue.
* Has potential to throw an exception.
	* `NoSuchElementException` if this queue is empty

-
-
### Queue API Structure<br>`poll()`
* Removes element at the head of the Queue.
* Throws no exceptions




-
## Queue API Structure: Viewing



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




-
-
### All About Queues
* Read more [here](http://www.codejava.net/java-core/collections/java-queue-collection-tutorial-and-examples)



-
# Puppies
