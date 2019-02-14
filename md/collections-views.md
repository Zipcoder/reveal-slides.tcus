# Collections Framework


-
## Collection Views & Wrappers



-
-
### List Wrapper
* `Array.asList` returns a `List` wrapper around a plain Java array.
* This allows the user to pass the array as a `List`.
* Any operation that would change the `size` of the `List` will throw an `UnsupportOperationException`.

### Views
* lightweight object which implements `Collection` or `Map` interface.
* is not a _real_ collection in a traditional sense.
* stores only references another collection, array or a single object and uses it to provide the data to a user.







-
-
### Views with exceptional mutators

* Each of the following views throws an `UnsupportedOperationException` upon attempting to mutate the data
<font size = 6>
* `Collection unmodifiableCollection(Collection)`* `List unmodifiableList(List)`* `Set unmodifiableSet(Set)`* `SortedSet unmodifiableSortedSet(SortedSet)`* `NavigableSet unmodifiableNavigableSet(NavigableSet)`* `Map unmodifiableMap(Map)`* `SortedMap unmodifiableSortedMap(SortedMap)`* `SortedMap unmodifiableNavigableMap(NavigableMap)`
</font>


-
-
### Views with synchronized methods
* Each of the following views locks shared resources across threads.
<font size = 6>
* `Collection synchronizedCollection(Collection)`* `List synchronizedList(List)`* `Set synchronizedSet(Set)`* `SortedSet synchronizedSortedSet(SortedSet)`* `NavigableSet synchronizedNavigableSet(NavigableSet)`* `Map synchronizedMap(Map)`* `SortedMap synchronizedSortedMap(SortedMap)`* `NavigableMap synchronizedNavigableMap(NavigableMap)`
</font>


-
-
### Views which validate elements for type before insertion
* Each of the following views' methods throw a `ClassCastException` if an element of the wrong type is inserted.
<font size = 6>
* `Collection checkedCollection(Collection, Class elementType)`* `List checkedList(List, Class elementType)`
* `Set checkedSet(Set, Class elementType)`
* `SortedSet checkedSortedSet(SortedSet, Class elementType)`
* `NavigableSet checkedNavigableSet(NavigableSet, Class elementType)`* `Map checkedMap(Map, Class keyType, Class valueType)`
* `SortedMap checkedSortedMap(SortedMap, Class keyType, Class valueType)`* `NavigableMap checkedNavigableMap(NavigableMap, Class keyType, ClassvalueType)`* `Queue checkedQueue(Queue, elementType)`
</font>






-
# Puppies
