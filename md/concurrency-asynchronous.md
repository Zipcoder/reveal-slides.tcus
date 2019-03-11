# Concurrency

-
-
## Threads
* Allows tasks to run in parallel
* Can share data between each other
* Less overhead to create and destroy than processes
* Threads are often scheduled according to some prioritization scheme





-
-
## Thread States


-
#### Thread States<br>New
* Denotes that `Thread` is _setting up_
* Denotes that `Thread` is not running

-
#### Thread States<br>Runnable
* Denotes that `Thread` is _may be running_
* Only state where running is possible

-
#### Thread States<br>Blocked, Waiting, Timed Waiting
* Denotes that `Thread` is inactive until thread-scheduler tells them to run.

-
#### Thread States<br>Terminated
* Denotes that `Thread` has finished running







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
#### Encountering `InterruptedException`
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
## Thread scheduler
* responsible for prioritizing thread execution.
* decides when each thread should be interacted with
* decides when each thread should idle


-
#### Asynchronous Threading
* Notice that Example 1 does not control the _thread scheduler_. Rather, the scheduler independently decides when to prioritize each thread execution.
* Asynchronous threading leads to unpredictably ordered execution.
