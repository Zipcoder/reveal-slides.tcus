# Collections Framework


-
-
## The Collection Interface

|||
| -------------------- | --------------------------------- |
| `add(E)`             | `addAll(Collection<? Extends E>)` |
| `remove(o)`          | `removeAll(Collection<?>)`        |
| `clear()`            | `removeIf(Predicate<? super E>)`  |
| `size()`             | `containsAll(Collection<?>)`      |
| `stream()`           | `iterator()`                      |
| `isEmpty()`          | `contains(Object)`                |
| `toArray()`          | `T[] toArray(T[])`                |
|||



-
-
### Collections and Maps

||||
| ---------- | ------------- | --------------- |
| ArrayList  | EnumSet       | EnumMap         |
| LinkedList | LinkedHashSet | LinkedHashMap   |
| ArrayDeque | PriorityQueue | WeakHashMap     |
| HashSet    | HashMap       | IdentityHashMap |
| TreeSet    | TreeMap       |                 |
||||

Note: `WeakHashMap` uses `WeakReference` objects as keys; can be GC'd if no other references remain
`IdentityHashMap` uses `System.identityHashCode()` and `==` for storage and retrieval

-
-
## Collections vs Maps

Collections hold a series of individual elements.  
Maps store associated pairs of objects (called key-value pairs). The key object is used to look up the value object. Sometimes called dictionaries or associative arrays.  
Both resize automatically (unlike Arrays).


-
### List types

- ArrayList - Quicker with random access to elements
- LinkedList - Quicker with removal/insertion of elements in the middle of the list

-
### Set types

- HashSet - keeps elements in hash order (fast, not predictable)
- TreeSet - keeps elements in ascending sorted order
- LinkedHashSet - provides insertion-order access to elements
- BitSet - Optimized for storing sequences of bits
- EnumSet - Optimized set for holding `enum` instances

Sets are often used to test for membership using the `contains()` method

-
### Queues, Dequeues, and Stacks

- Queue -- Adds elements to one end and removes from the other (FIFO)
- Stack -- Adds elements to one end and removes them from the same (LIFO)
- Dequeue -- "double-ended queue" adds and removes elements at both ends

Note: `Dequeue` is often used for stacks as well


-
## Map types

- TreeMap - Keeps keys in insertion order
- HashMap - Hashes keys for quick access
- LinkedHashMap - Hashes keys, but preserves order with a LinkedList
- WeakHashMap - Objects stored in `WeakHashMap`s can still be garbage collected

-
## Using maps

- Elements are pairs of objects, called keys and values
- keys are used to lookup values
- Check for a specific key with `conainsKey(key)`
- Add key-value pairs with `put(key, value)`
- Get values with `get(key)`
- Keys are stored in a Set, retrievable with `keySet()`

-
-
## Collections Framework Part 2

-
-
## Iterators

Provides a way to iterate through a Collection.  
Implicitly used in foreach loops.  
Methods:

- `hasNext()` - returns true if there are more elements available
- `next()` - return the next element
- `remove()` - remove the last element returned from the underlying Collection

-
## ListIterators

Slightly more features than Iterators:

- Can iterate backward
- Provides index for previous and next elements


-
-
## Utility Classes

- [Collections](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html)
- [Arrays](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)
