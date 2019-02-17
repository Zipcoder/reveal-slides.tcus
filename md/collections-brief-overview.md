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




-
-
## Utility Classes

- [Collections](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html)
- [Arrays](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)
