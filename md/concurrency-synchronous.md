
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
