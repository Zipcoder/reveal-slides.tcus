# Concurrency

-
-
## Threads
* Allows tasks to run in parallel
* Share data between each other
* Less overhead to create and destroy than processes

-
## Thread implementation
* Done by implementing the `Runnable` interface
  * Functional interface with only a `void run();` in it
* `Thread t = new Thread(r);` where `r` is `Runnable`
* Thread is started by calling `t.start();`

-
```java
public class Example {
    public static void main(String[] args) {
        Runnable forrestGump = new Threader();
        Thread t = new Thread(forrestGump);
        t.run();
    }
}
class Threader implements Runnable {
    public void run() {
        System.out.println("I WAS RUNNING!");
    }
}
```

-
-
## Thread Interruption

-
* Threads normally terminate when their `run` method returns
* `interrupt` requests early termination, but is not guaranteed
* Up to the `run` method to handle `interrupt` calls correctly
* If a thread is sleeping, then the `interrupt` throws an InterruptedException.

-
```java
public class Example {
    public static void main(String[] args) {
        Runnable runner = new Threader();
        Thread t = new Thread(runner);
        t.start();
        try {
            Thread.sleep(5000);
            System.out.println("I'M GONNA STOP THE THREAD NOW!");
        } catch(InterruptedException e) {
            System.out.println("main somehow got interrupted");
        }
        t.interrupt();
    }
}
class Threader implements Runnable {
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            System.out.println("I AM RUNNING!");
        }

    }
}
```

-
```java
public class Example {
    public static void main(String[] args) {
        Runnable runner = new Threader();
        Thread t = new Thread(runner);
        t.start();
        try {
            Thread.sleep(5000);
            System.out.println("I'M GONNA STOP THE THREAD NOW!");
        } catch(InterruptedException e) {
            System.out.println("main somehow got interrupted");
        }
        t.interrupt();
    }
}
class Threader implements Runnable {
    public void run() {

        try {
            while(true) {
                System.out.println("I AM RUNNING!");
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            System.out.println("WHO DISTURBS MY SLUMBER!");
        }
    }

}
```

-
## Thread States
* new => Set up.  Not Running.
* runnable => Might be running.  Only state where running is possible.
* blocked, waiting, timed waiting => Inactive until the schedule tells them to run
* terminated => Finished running

-
-
## Thread Synchronization

-
## Race Conditions
A race condition is a situation where multiple threads have access to the same object and try to modify that object.

-
```java
public class Example {
    public static void main(String[] args) {
        Threader t = new Threader();
        Thread first = new Thread(t);
        Thread second = new Thread(t);
        first.start();
        second.start();

        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            System.out.println("INTERRUPTED");
        }
        System.out.println(t.sharedInt);
    }
}

class Threader implements Runnable {
    int sharedInt = 0;

    public void run() {
        for(int i = 0; i < 1_000_000; i++) {
            sharedInt = sharedInt + 1;
        }
        System.out.println("THREAD FINISHED -- + " + Thread.currentThread());
    }
}
```

-
In Java, some of the ways to protect against race conditions are:
* Locks
* `syncronized` keyword (which is an intrinsic lock)

For now we'll just look at explicitly locking things.

-
```java
class Threader implements Runnable {
    int sharedInt = 0;
    Lock sharedLock = new ReentrantLock();

    public void run() {
        for(int i = 0; i < 1_000_000; i++) {
            sharedLock.lock();
            sharedInt = sharedInt + 1;
            sharedLock.unlock();
        }
        System.out.println("THREAD FINISHED -- + " + Thread.currentThread());
    }
}
```

-
## Deadlocks
When two or more threads are waiting for the other to release a lock.