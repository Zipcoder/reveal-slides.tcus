# Thread Synchronization




-
-

## What we will cover this lecture
<ul>
	<li class="fragment fade-up">Race Conditions</li>
	<li class="fragment fade-up">Preventing race conditions
  	<ul>
  	     <li class="fragment fade-up">explicit mutual exclusion locking
        <ul>
            <li class="fragment fade-up">`ReentrantLock` objects</li>
        </ul>
        </li>
        <li class="fragment fade-up">implicit monitor locking
            <ul>
                <li class="fragment fade-up">`Synchronized` signature clause</li>
                <li class="fragment fade-up">`Synchronous` blocks</li>
            </ul>
        </li>
    </ul>
    <li class="fragment fade-up">Deadlocks</li>
</ul>















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
        System.out.println(runnable.getCount());
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
  * Data is considered _stale_ when the value of an operand changes but fetching the operand obtains the old, rather than the new, value of the operand

















-
-
## Preventing Race Conditions
















-
-
#### ReentrantLock Overview
* Also known as _mutual exclusion Lock_
* `java.util.concurrent.locks.ReentrantLock` is a built-in java-class that provides synchronization to methods while accessing shared resources.

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

Let's put together a situation that would cause a deadlock with our threads...

-
### Encountering a Deadlock: ThreadDemo1



```java
public class ThreadDemo1 extends Thread {

  public static Object lock1;
  public static Object lock2;

  public ThreadDemo1(Object lock1, Object lock2) {
    this.lock1 = lock1;
    this.lock2 = lock2;
  }

  public void run() {
    synchronized (lock1) {
      System.out.println("Thread 1: Holding lock 1...");
      try {
        Thread.sleep(10);
      } catch (InterruptedException e) {}
        System.out.println("Thread 1: Waiting for lock 2...");

      synchronized (lock2) {
        System.out.println("Thread 1: Holding lock 1 & 2...");
      }
    }
  }
}

```




-
### Encountering a Deadlock: ThreadDemo2

```java
public class ThreadDemo2 extends Thread {

  public static Object lock1;
  public static Object lock2;

  public ThreadDemo2(Object lock1, Object lock2) {
    this.lock1 = lock1;
    this.lock2 = lock2;
  }

  public void run() {
    synchronized (lock2) {
      System.out.println("Thread 2: Holding lock 2...");
      try {
        Thread.sleep(10);
      } catch (InterruptedException e) {}
      System.out.println("Thread 2: Waiting for lock 1...");

      synchronized (lock1) {
          System.out.println("Thread 2: Holding lock 1 & 2...");
      }
    }
  }
}

```


-

### Encountering a Deadlock
* Run the Demo:

```java
public class AsynchDemo {
    public static Object lock1 = new Object();
    public static Object lock2 = new Object();

    public static void main(String[] args) {
        ThreadDemo1 T1 = new ThreadDemo1(lock1, lock2);
        ThreadDemo2 T2 = new ThreadDemo2(lock1, lock2);
        T1.start();
        T2.start();
    }
}
```
*Output: 

```
Thread 1: Holding lock 1...
Thread 2: Holding lock 2...
Thread 1: Waiting for lock 2...
Thread 2: Waiting for lock 1...
```

-

### Encountering a Deadlock : Fix



To fix this deadlock, we'll change the order of the locks on one of our ThreadDemo classes.




-
### Fixing a Deadlock: ThreadDemo1

(unchanged)

```java
public class ThreadDemo1 extends Thread {

  public static Object lock1;
  public static Object lock2;

  public ThreadDemo1(Object lock1, Object lock2) {
    this.lock1 = lock1;
    this.lock2 = lock2;
  }

  public void run() {
    synchronized (lock1) {
      System.out.println("Thread 1: Holding lock 1...");
      try {
        Thread.sleep(10);
      } catch (InterruptedException e) {}
        System.out.println("Thread 1: Waiting for lock 2...");

      synchronized (lock2) {
        System.out.println("Thread 1: Holding lock 1 & 2...");
      }
    }
  }
}

```


-
### Fixing a Deadlock: ThreadDemo2

(changed the order of the locks on ThreadDemo2)

```java
public class ThreadDemo2 extends Thread {

  public static Object lock1;
  public static Object lock2;

  public ThreadDemo2(Object lock1, Object lock2) {
    this.lock1 = lock1;
    this.lock2 = lock2;
  }

  public void run() {
    synchronized (lock1) {
      System.out.println("Thread 2: Holding lock 2...");
      try {
        Thread.sleep(10);
      } catch (InterruptedException e) {}
      System.out.println("Thread 2: Waiting for lock 1...");

      synchronized (lock2) {
          System.out.println("Thread 2: Holding lock 1 & 2...");
      }
    }
  }
}

```
-


### Encountering a Deadlock
* Now, run the Demo:

```java
public class AsynchDemo {
    public static Object lock1 = new Object();
    public static Object lock2 = new Object();

    public static void main(String[] args) {
        ThreadDemo1 T1 = new ThreadDemo1(lock1, lock2);
        ThreadDemo2 T2 = new ThreadDemo2(lock1, lock2);
        T1.start();
        T2.start();
    }
}
```
 * Output: 

```
Thread 1: Holding lock 1...
Thread 1: Waiting for lock 2...
Thread 1: Holding lock 1 & 2...
Thread 2: Holding lock 2...
Thread 2: Waiting for lock 1...
Thread 2: Holding lock 1 & 2...

Process finished with exit code 0

```

-
-


<img src="https://github.com/Zipcoder/reveal-slides.tcus/blob/master/img/bunnies/baby-bunnies.jpg?raw=true">

