# Locks in java
A lock is a thread synchronization mechanism like synchronized bloocks expect lock can be more sophisitcated than Java synchronized blocks. Locks(and other more advanced synchronization mechanisms) are created using synchronized blocks, so it is not like we can get totally rid of the synchronized keyword.

From Java 5 the package `java.util.concurrent.locks` contains several lock implementation, so you may not have to implement your own locks. But you will still need to know how to use them and it can still be useful to know the theory behind their implementation. 

# Main difference between a Lock and a Synchronized block
The main differences between a Lock and a synchronized block are:
- A synchronized block makes no guarantees about the sequence in which threads waiting to entering it are granted access.
- You cannot pass any parameter to the entry of synchronized block. This, having a timeout trying to get access to a synchronized block is not possible.
- The synchronized block must be fully contained with a single method. A Lock can have it's call to lock() and unlock() in separate methods.

# A Simple Lock
Let's start out by looking at a synchronized block of Java code:
```java
public class Counter {
	private int count = 0;

	public int inc() {
		synchronized(this) {
			return ++count;
		}
	}
}
```
> this synchronized(this) block in the inc() method. This block makes sure that only one thread can execute the return ++count at a time. The code in the synchronized block could have been more advanced, but the simple ++count suffices to get the point across.

The Counter class could have been written like this instead, using a Lock instead of synchronized block:

```java
public class Counter {

	private Lock lock = new Lock();
	private int count = 0;

	public int inc() {
		lock.lock();
		int newCount = ++count;
		lock.unlock();
		return newCount;
	}
}
```
The lock() method locks the Lock instance so that all thread calling lock() are blocked until unlocl() is executed.

Here is a simple Lock implementation:

```java
public class Lock {
	private boolean isLocked = false;

	public synchronized void lock() throws InterruptedException{
		while(isLocked) {
			wait();
		}
		isLocked = ture;
	}

	public synchronized void unlock() {
		isLocked = false;
		notify();
	}
}
```
Notice the `while(isLocked)` loop, which is also called a _spin lock_. While `isLocked` is true, the thread calling lock() is parked  waiting in the `wait()` call. In case the thread should return unexpectedly from the wait() call without having received a notify() call the thread re-checks the `isLocked` condition to see if it is safe to proceed or not, rather than just assume that being awakened means it is safe to proceed. If `isLocked` is false, the thread exits the `while(isLocked)` loop, and set `isLocked` is false, the thread exists the `while(isLocked)` loop, and sets `isLocked` back to true, to lock the Lock instance for other thread calling `lock()`.

When the thread is done with the code in the critical section(the code between lock()) and unlock(), the thread call unlock. Executing unlock() set isLocked back to false, and notifies (awakens) on of the thread waiting in the wait() call in the lock() method, if any.

# Lock Reentrance
Synchronized block in Java reentrant. This means, that if a Java thread enters a synchronized block of code, and thereby take the lock on the monitor object the block is synchronized on, the thread can enter other Java code blocks synchronized on the same monitor object. Here is an example:

```java
public class Reentrant {

	public synchronized outer() {
		inner();
	}

	public synchronized inner() {
		//do something
	}
}
```

Notice how both `outer()` and `inner()` are declared synchronized, which in Java is equivalent to a `synchronized(this)` block. If thread calls `outer()` there is no problem calling `inner()` from inside `outer()`, since both methods (or blocks) are synchronized on the same monitor object (this). If a thread already holds the lock on monitor object, it has access to all blocks synchronized on the same monitor object. This is called reentrance. The thread can reenter any block of code for which it already holds the lock.

The lock implementation shown earilier is not reentrant. If we write the Reentrant class like below, the thread calling `outer()` will be block inside the lock.lock() in the `inner()` method.

```java
public class Reentrant2 {

	Lock lock = new Lock();

	public void outer() {
		lock.lock();
		inner();
		lock.unlock();
	}

	public void inner() {
		lock.lock();
		//do something
		lock.unlock();
	}
}
```
A thread calling `outer()` will first lock the Lock instance. Then it will call `inner()`. Inside the `inner()` method the thread will again try to lock the Lock instance. This will fail(meaning the thread will be blocked), since the Lock instance was locked already in the outer() method.

The reason the thread will be blocked the second time it calls lock() without having called unlock in between, is apparent when we look at the lock() implementation.

```java
public class Lock{

  boolean isLocked = false;

  public synchronized void lock()
  throws InterruptedException{
    while(isLocked){
      wait();
    }
    isLocked = true;
  }

  ...
}
```
It is the condition inside the while loop (spin lock) that determines if a thread is allowed to exit the lock method or not. Currently the condition is that isLocked must be false for this to be allowed, regardless of what thread lock it.

To make the Lock class reentrant we need to make a small change:
```java
public class Lock{

  boolean isLocked = false;
  Thread  lockedBy = null;
  int     lockedCount = 0;

  public synchronized void lock()
  throws InterruptedException{
    Thread callingThread = Thread.currentThread();
    while(isLocked && lockedBy != callingThread){
      wait();
    }
    isLocked = true;
    lockedCount++;
    lockedBy = callingThread;
  }


  public synchronized void unlock(){
    if(Thread.curentThread() == this.lockedBy){
      lockedCount--;

      if(lockedCount == 0){
        isLocked = false;
        notify();
      }
    }
  }

  ...
}
```
Notice how the while loop now also takes the thread that locked the Lock instance into consideration. If either the lock is unlocked(isLocked = false) or the calling thread is the thread that locked the Lock instance, the while loop will not execute, and thread calling lock() will be allowed to exit the method.

Additionall, we need to count the number of times the lock been locked by the same thread. Otherwise a single call to unlock() will unlock the lock, even if the lock has been locked multiple times. We don't want the lock to be unlocked until the thread that locked it, has executed the same amount of unlock() calls as lock() calls.

The Lock class is now reentrant.

# Lock fairness
Java's synchronized block makes no guarantess about the sequence in which threads trying to enter them are granted access. Therefore, if many threads are constantly competing for access to the same synchronized block, there is a risk that one or more of the threeads are never granted access - that access is always granted to other threads. This called starvation. To avoid this a Lock should be fait. Since the Lock implementation shown in this text uses synchronized blocks internally, they do not guarantee fairness. 

# Calling unlock() From a finally-clause
When guarding a critical section with a Lock, and the critical section may throw exceptions, it is important to call the unlock() method from inside a finally-clause. Doing so makes sure that the Lock is unlocked so other thread can lock it. Here is an example:

```java
lock.lock();
try{
  //do critical section code, which may throw exception
} finally {
  lock.unlock();
}
```
This little construct makes sure that the Lock is unlocked in case an expection is thrown from code in the critical section. If unlock() was not called from inside a finally-clause, and an exception was thrown from the critical section, the Lock would remain locked forever, causing all thread calling lock() on that Lock instance to halt indefinately.


**References**: 
- [http://tutorials.jenkov.com/java-concurrency/locks.html](http://tutorials.jenkov.com/java-concurrency/locks.html)
