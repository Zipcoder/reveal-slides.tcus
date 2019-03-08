

-
-
# Behavioral Patterns
* Characterize the ways in which classes or objects interact and distribute responsibility.
* Concerned with algorithms and the assignment of responsibilities between objects
* Describes object / class patterns as well as the resulting communication patterns between them
* Some Examples:
	* Template
	* Observer
	* Command

-
### Template Pattern
* Create a method of high degree of freedom to define methods of lesser variability
	* _Degree of freedom_ is determined by the number of arguments of a method

```java
// degree of 1
public static Integer[] getRange(int start) {
    return getRange(0, start, 1);
}

// degree of 2
public static Integer[] getRange(int start, int stop) {
    return getRange(start, stop, 1);
}

// degree of 3; template method
public static Integer[] getRange(int start, int stop, int step) {
    List<Integer> list = new ArrayList<>();
    for(int i = start; i<stop; i+=step) {
        list.add(i);
    }
    return list.toArray(new Integer[list.size()]);
}
```


-
-
## Observer Pattern
* Register observer (subject) to a observable
* Remove observer from observable
* Notify observer on state change of observable


-
### Observable
* Responsible for notifying registered observer's of state-change

```java
public interface Observable<T extends Observer> {
	void register(T observer);
	void unregister(T observer);
	void notify()
	List<Observer> getAllObservers();
}
```


-
### What is an Observer?
* Responsible for being updated upon Observable state-change
```java
public interface Observer {
	void update();
}
```


-
## Command Pattern
* Command interface
	* Decoupling "what is done" from "when it is done"
	* Concrete commands objects are tasks; i.e. - `UNDO_TASK`

-
## Command Pattern
You'll see command being used a lot when you need to have multiple undo operations, where a stack of the recently executed commands are maintained. To implement the undo, all you need to do is get the last Command in the stack and execute it's undo() method.

You'll also find Command useful for wizards, progress bars, GUI buttons and menu actions, and other transactional behavior.  
