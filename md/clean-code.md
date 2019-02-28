#Clean Code



-
-
# Clean Code
Any fool can write code a system can understand. Good programmers write code that humans can understand.
- Martin Fowler

-
-
# Be predictable!
# No Side Effects!






-
-
# Meaningful Descriptive Names
- Classes should be nouns
- Methods should be verbs
- Descriptive name over detailed comments






-
# Meaningful Descriptive Names
```java
int d; // elapsed time in days
```
VS

```java
int elapsedTimeInDays;
```





-
# Meaningful Descriptive Names
What is this doing?

```java
public List flags() {
   List list1 = new ArrayList();
   for (int[] x : this.theList){
    if (x[0] == 4) {
    list1.add(x);
    }
   }
   return list1;
}
```
-
# Meaningful Descriptive Names
* What is this doing?
```java
public List getFlaggedCells() {
   List flaggedCells = new ArrayList();
   for (int[] cell : this.gameBoard) {
      if (cell[STATUS_VALUE] == FLAGGED) {
        flaggedCells.add(cell);
      }
   }
   return flaggedCells;
}

```

-
-

# Single Responsibility
- Do one thing
  - variable
  - method
  - class
  - test




-
# Small methods
- Do one thing (SRP)
  - change the state of an object
  - get information about that object
- Less than 20 lines
- Maximum of 3 params

-
-
# One word per concept
- Length / size
- fetch / retrieve / get

-
-

# Testable
- Write the test, then write to code to make the test pass
- Tests should be
    - Fast
    - Independent - no test should depend on another test
    - Repeatable - can run in any environment
    - Self-validating (assert pass or fail, no manual check)
    - Timely (don’t write your test too late)

-
-
# Don’t repeat yourself! (DRY)
Sharing is caring


-
-
# Comments
- Express yourself in code
- Use comments to explain why not
  ```java
    // This test is extremely slow, only run it when you have time
  ```
- Use comments to warn others
  ```java
    // This makes an actual call to production API
  ```

-
-
# Formatting
- Read top to bottom
- Constants, instance variable, methods
- Related concepts should be close to each other
- Variables should be declared as close to their usage as possible

-
-
# Encapsulation
- A mechanism of wrapping the data (variables) and code acting on the data (methods) together as a single unit

-
-
# Conclusion
- Write readable code
- Meaningful names
- Write the test, write the code to make it work, then clean it!
- Do one thing and do it well (Single Responsibility Principle)
- No side effect
- DRY (Don’t Repeat Yourself)
- KISS (Keep It Simple, Stupid)

-
-
Follow the boyscout rule:
# “Leave the code cleaner than you found it.”
