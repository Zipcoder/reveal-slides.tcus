# Thread Synchronization








-
-
## What we will cover this lecture
* Deadlocks
* Race Conditions
* Preventing race conditions
  * explicit mutual exclusion locking
    * `ReentrantLock` objects
  * implicit monitor locking
    * `Synchronized` signature clause
    * `Synchronous` blocks
















-
-
## Race Conditions
* Situation in which multiple threads have access to the same object and try to modify that object without checking if its used by other threads.
* Can cause `ConcurrentModificationException` to be thrown

-
#### Defining a Runnable Class
* Consider an application in which, multiple threads are responsible for mutating a single object-state across multiple `Thread` objects.
```java
public class AsynchCounter implements Runnable {
    private int count = 0;

    public void run() {
        for(int i = 0; i < 1_000_000; i++) {
            count = count + 1;
        }
        String currentThread = Thread.currentThread().toString();
        System.out.println("THREAD FINISHED: " + currentThread);
    }
}
```

-
#### Using Runnable to Construct Thread
```java
public class MainApplication {
    public static void main(String[] args) {
        AsynchCounter runnable = new AsynchCounter();
        Thread first = new Thread(runnable);
        Thread second = new Thread(runnable);
        first.start();
        second.start();

        ThreadUtils.sleep(5000); // sleep 5 seconds
        System.out.println(runnable.getcount());
    }
}
```

Output
```
THREAD FINISHED: Thread[Thread-0,5,main]
THREAD FINISHED: Thread[Thread-1,5,main]
1131532
```



-
#### Identifying Race conditions
* Notice that the output of the previous program was a value greater than 1,000,000.
* This is caused by threads accessing _stale_ data.
  * Data is considered _stale_ the value of an operand changes but fetching the operand and obtains the old rather than the new value of the operand

















-
-
## Preventing Race Conditions
















-
-
#### ReentrantLock Overview
* Also known as _mutual exclusion Lock_
* `java.util.concurrent.locks.ReentrantLock` is a built-in java-class that provides synchronization to methods while accessing shared resources.
* Key features:
  * Ability to lock interruptibly
  * Ability to timeout while waiting for lock
  * Ability to create fair lock
  * API to get list thread of waiting for lock
  * Flexibility to try for lock without blocking

-
#### ReentrantLock Tidbits
* are owned by the thread last successfully locking, but not yet unlocking it.
* thread invoking lock will return when lock is not owned by another thread.
* method will return immediately if current thread already owns the lock.








-
-
### Creating Reentrant Structures
* Consider an application in which, multiple threads are responsible for mutating a single object-state across multiple `Thread` objects.

```java
public class ReentrantCounter implements Runnable {
    private int count = 0;
    private ReentrantLock lock;

    public ReentrantCounter(ReentrantLock lock) {this.lock = lock;}

    public void run() {
        lock.lock(); // thread resources locked
        for(int i = 0; i < 1_000_000; i++) {
            count = count + 1;
        }
        String currentThread = Thread.currentThread().toString();
        System.out.println("THREAD FINISHED: " + currentThread);
        lock.unlock();  // thread resources unlocked
    }
}
```


-
#### Using Reentrant Structures
* Notice the predictable output of `demo`

```java
public void demo() {
    ReentrantLock lock = new ReentrantLock();
    ReentrantCounter runnable = new ReentrantCounter(lock);
    Thread first = new Thread(runnable);
    Thread second = new Thread(runnable);
    first.start();
    second.start();

    ThreadUtils.sleep(5000); // sleep 5 seconds
    System.out.println(runnable.getCount());
}
```

Output
```
THREAD FINISHED: Thread[Thread-0,5,main]
THREAD FINISHED: Thread[Thread-1,5,main]
2000000
```





-
-
### Creating Reentrant Structures
* Consider the case that `ReentrantCounter` never calls `unlock` on its `ReentrantLock` field.

```java
public class ReentrantCounter implements Runnable {
    private int count = 0;
    private ReentrantLock lock;

    public ReentrantCounter(ReentrantLock lock) {this.lock = lock;}

    public void run() {
        lock.lock(); // locked
        for(int i = 0; i < 1_000_000; i++) {
            count = count + 1;
        }
        String currentThread = Thread.currentThread().toString();
        System.out.println("THREAD FINISHED: " + currentThread);
        // never unlocked
    }
}
```


-
#### Using Reentrant Structures
* Notice the output of `demo`

```java
public void demo() {
    ReentrantLock lock = new ReentrantLock();
    ReentrantCounter runnable = new ReentrantCounter(lock);
    Thread first = new Thread(runnable);
    Thread second = new Thread(runnable);
    first.start();
    second.start();

    ThreadUtils.sleep(5000); // sleep 5 seconds
    System.out.println(runnable.getCount());
}
```

Output
```
THREAD FINISHED: Thread[Thread-0,5,main]
THREAD FINISHED: Thread[Thread-1,5,main]
1000000
```





























-
-
## Synchronized Clause Overview
* `synchronized` is declared in the _signature_ of a synchronous method.
* allows a method to be _tagged_ as a synchronous operation
  * this enforces that a method is wrapped by an implicit `ReentrantLock.lock()` and `ReentrantLock.unlock()` method call.

-
-
### Synchronized Clause Declaration
Instance method declaration
```java
public synchronized Integer doComputation() {
  // definition omitted
}
```

Static method declaration
```java
public static synchronized Integer doComputation() {
  // definition omitted
}
```


-
### Creating Synchronized Methods
* Consider an application in which, multiple threads are responsible for mutating a single object-state across multiple `Thread` objects.

```java
public class SynchronizedCounterr implements Runnable {
    private int count = 0;

    public void run() {
        for(int i = 0; i < 1_000_000; i++) { incrementCount(); }
        String currentThread = Thread.currentThread().toString();
        System.out.println("THREAD FINISHED: " + currentThread);
    }

    public synchronized void incrementCount() { this.count+=1; }

    public int getCount() { return count; }
}
```


-
#### Using Synchronized Methods
* Notice the predictable output of `demo`

```java
public void demo() {
    SynchronizedCounter runnable= new SynchronizedCounter();
    Thread first = new Thread(runnable);
    Thread second = new Thread(runnable);
    first.start();
    second.start();

    ThreadUtils.sleep(5000); // sleep 5 seconds
    System.out.println(runnable.getCount());
}
```


Output
```
THREAD FINISHED: Thread[Thread-0,5,main]
THREAD FINISHED: Thread[Thread-1,5,main]
2000000
```



















-
-
## Synchronous Block
* `synchronous` keyword (which is an intrinsic lock)
* Useful when we want to synchronize part of method, but not all of it.
* Takes an argument of the object to be locked before allowing other thread access.





-
### Creating Synchronized Blocks
* Notice use of the `synchronized` block

```java
public class SynchronousCounter implements Runnable {
    private int count = 0;
    public void run() {
        for(int i = 0; i < 1_000_000; i++) {
            incrementCount();
        }
        String currentThread = Thread.currentThread().toString();
        System.out.println("THREAD FINISHED: " + currentThread);
    }

    public void incrementCount() {
        synchronized (this) {
            count += 1;
        }
    }
}
```


-
#### Using Synchronized Blocks
* Notice the predictable output of `demo`

```java
public void demo() {
    SynchronousCounter runnable= new SynchronousCounter();
    Thread first = new Thread(runnable);
    Thread second = new Thread(runnable);
    first.start();
    second.start();

    ThreadUtils.sleep(5000); // sleep 5 seconds
    System.out.println(runnable.getCount());
}
```

























-
-
## Deadlocks
* When two or more threads are waiting for the other to release a lock.


-
### Encountering a Deadlock
* Blah

```java
public void demo() {

}
```
