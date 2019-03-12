# Concurrency

## Concurrency
* multiple sequences of operations are run in overlapping periods of time
* distinct from _parallelism_
  * computational task that is broken down into several, often many, very similar sub-tasks, which can be processed independently and whose results are combined afterwards, upon completion.
  * _merge sort_ often has a _parallel_ implementation.



-
-
## Thread Object
* Allows tasks to run in parallel
* Can share data with other `Thread` instances
* Less overhead to create and destroy than processes
* Threads are often _scheduled_ according to some prioritization scheme


-
## Thread scheduler
* responsible for prioritizing thread execution
* decides when each thread should be active
* decides when each thread should be idle




-
-
## Thread States


-
#### Thread States<br>New
* Indicates that `Thread` is _setting up_
* Indicates that `Thread` is not running

-
#### Thread States<br>Runnable
* Indicates that `Thread` _may be running_
* Only state where running is possible

-
#### Thread States<br>Blocked, Waiting, Timed Waiting
* Indicates that `Thread` is inactive until _thread-scheduler_ tells them to run.


-
#### Thread States<br>Terminated
* Indicates that `Thread` has finished running







-
## Runnable Interface
* `Thread` execution is dependent on a `Runnable` object.
  * A thread cannot `run` without a `Runnable` instance to say _how to_ run.
* To construct a new `Thread`, one must provide the thread with a `Runnable` upon construction.

```java
public void demo() {
  Runnable runnable = ()->System.out.println("Hello world!");
  Thread thread = new Thread(runnable);
  thread.start(); // begins an independent thread of execution
}
```





-
-
## Thread Interruption
* `Thread` typically terminates upon completing the `run` method of the respective `Runnable`.
* `interrupt` requests early termination, but is not guaranteed
* `run` is responsible for handling `interrupt` calls correctly.
* `interrupt` throws an `InterruptedException`, if a thread is sleeping.

-
#### Delaying Execution
* Tells program to cease execution on current thread for 5 seconds (5000 milliseconds).

```java
public void demo() {
  try {
    Thread.sleep(5000);
  } catch(InterruptedException e) {
    e.printStackTrace();
  }
}
```

-
#### Encapsulating `InterruptedException`
* Defers `try` / `catch` to a separate procedure
  * (A tool for future examples in this lecture)

```java
public class ThreadUtils {
  public static void sleep(Integer milliseconds) {
    try {
      Thread.sleep(milliseconds);
    } catch(InterruptedException e) {
      e.printStackTrace();
    }
  }
}
```





-
#### Thread Interruption<br>Example 1
```java
public void demo() {
    Runnable runnable = this::execute;
    Thread t = new Thread(runnable);
    t.start();
    ThreadUtils.sleep(5000);

    System.out.println("I'M GONNA STOP THE THREAD NOW!");
    t.interrupt();
}

public void execute() {
    while (!Thread.currentThread().isInterrupted()) {
        System.out.println("I AM RUNNING!");
    }
}
```

Output
```
I AM RUNNING!
I AM RUNNING!
I'M GONNA STOP THE THREAD NOW!
I AM RUNNING!
I AM RUNNING!
```


-
#### Asynchronous Threading
* From output of Example 1, notice the lack of control of the _thread scheduler_. The scheduler independently decides when to prioritize each thread execution.
* Asynchronous threading leads to unpredictably ordered execution.
