# Collections<br>Stack, LinkedList, Queue





-
-
### Stack Class
* pop
* push
* peek
* isEmpty


-
### Short comings of Java's Stack class
* Is a subclass of `Vector` is a subclass of `List`
* Should have been interface








-
-
### `isEmpty`
* returns true if `Stack` contains no elements


-
### `isEmpty` example

```java
@Test
public void demo() {
		Stack<String> stack = new Stack<>();
		System.out.println(stack.isEmpty()); // prints true
}
```

Output
```
true
```





-
-
### `push`
* populates the `Stack` with the respective argument
* newly inserted element is considered to be the `head` or top-of-stack



-
### `push` example

```java
@Test
public void demo() {
		Stack<String> stack = new Stack<>();
		stack.push("Hello world");
		System.out.println(stack.isEmpty()); // prints false
}
```

Output
```
false
```








-
-
### `peek`
* views the most recently added item

-
### `peek` example

```java
@Test
public void demo() {
		Stack<String> stack = new Stack<>();
		stack.push("Hello world");
		System.out.println(stack.peek());
}
```

Output
```
Hello world
```







-
-
### `pop`
* removes and returns the most recently added element

-
### `pop` example
```java
@Test
public void demo() {
		Stack<String> stack = new Stack<>();
		System.out.println(stack.isEmpty()); // prints true
		stack.push("Hello world");
		System.out.println(stack.isEmpty()); // prints false
		String topValue = stack.pop();
		System.out.println(topValue); // throws EmptyStackException
}
```

Output
```
true
false
java.util.EmptyStackException
	at java.base/java.util.Stack.peek(Stack.java:102)
```












-
-
## LinkedList
* `LinkedList` is quicker than `ArrayList` at removal/insertion of elements in the middle of the list.
* `LinkedList` values are stored as `Node` objects.
	* Each `Node` is a separate object with a `data` and `next` field.



-
### Node
```
class Node<DataType> {
	DataType data;
	Node next;

	Node(DataType d) {
		data = d;
		next = null;
	}
}
```





-
### LinkedList

```java
class LinkedList<DataType> {
  Node<DataType> head;
}
```


-
### LinkedList
* Iterating a linked list
* Requires client to continually check if `next` is null, if not `this.head = next`











-
-
# Queue Interface
* Specifies that you can
	* add elements at the tail end of the queue,
	* remove elements at the head,
	* find out how many elements are in the queue.


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
### Queues, Dequeues, and Stacks

- Queue -- Adds elements to one end and removes from the other (FIFO)
- Stack -- Adds elements to one end and removes them from the same (LIFO)
- Dequeue -- "double-ended queue" adds and removes elements at both ends

Note: `Dequeue` is often used for stacks as well


-
-
### All About Queues
* Read more [here](http://www.codejava.net/java-core/collections/java-queue-collection-tutorial-and-examples)



-
# Puppies
