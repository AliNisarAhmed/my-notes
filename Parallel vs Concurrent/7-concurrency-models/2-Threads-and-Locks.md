
Despite their well-known problems, threads and locks are still the default
choice for writing much concurrent software, and they underpin many of the
other technologies we’ll be covering. Even if you don’t plan to use them
directly, you should understand how they work.

# Mutual Exclusion & Memory Model

## Mutual Exclusion

*mutual exclusion* - using locks to ensure that only one thread can access data at a time.
- mutual exclusion can go wrong including race conditions and deadlocks


## Threads

Basic unit of concurrency in Java, which encapsulates a single *thread of control*

Threads communicate with each other via shared memory.


```java
public class HelloWorld {
	public static void main(String[] args) throws InterruptedException {
		Thread myThread = new Thread() {
			public void run() {
				System.out.println("Hello from new thread");
				}
			};
			
		myThread.start();
		Thread.yield(); 
processor.
		System.out.println("Hello from main thread");
		myThread.join();
	}
}

// Thread.yield = a hint to the scheduler that the current thread is willing to yield its current use of a processor
// Without this call, the startup overhead of the new thread would mean that the main thread would almost certainly get to its println() first
```

The output of the above program could be in any order (non-determinism)


## Locks

Locks can be held by a single thread at any one time


### Intrinsic locks

Built-in locks in Java with the `synchronized` keyword come built into every Java object (aka mutex, monitor or critical section)


```java
class Counter {
	private int count = 0;
	public synchronized void increment() { ++count; }
	public int getCount() { return count; }
}
```
^ calling `increment()` claims the `Counter` object's lock which releases it when it returns, so only 1 thread can execute its body at a time.
- Any other thread that calls `increment`  will *block* until the lock becomes free

## Memory Visibility

The Java memory model defines when changes to memory made by one thread become visible to another thread.1 The bottom line is that there are no guarantees unless both the reading and the writing threads use synchronization.

An important point that’s easily missed is that both threads need to use synchronization. It’s not enough for just the thread making changes to do so.


## Deadlocks

Whenever a thread tries to hold more than 1 lock, deadlocks become a possibility
- Dining philosophers is a classic example

Happily, there is a simple rule that guarantees you will never deadlock—always
acquire locks in a fixed, *global order.*

Also ensure to not call alien methods (functions in external code) while holding a lock
- since that function may itself be using locks, hence possibility of deadlocks

Locks should be held for the shortest possible amount of time to avoid deadlocks


## Java Memory Model

Read: [William Pughs FAQs on Memory Model](https://www.cs.umd.edu/~pugh/java/memoryModel/jsr-133-faq.html)



## Questions:

- What guarantees does the Java memory model make regarding initialization safety? Is it always necessary to use locks to safely publish objects between threads?

*Initialization safety*: If an object is properly constructed (which means that references to it do not escape during construction), then all threads which see a reference to that object will also see the values for its final fields that were set in the constructor, without the need for synchronization.

- What does synchronization do?

	- The most well-understood is mutual exclusion -- only one thread can hold a monitor at once, so synchronizing on a monitor means that once one thread enters a synchronized block protected by a monitor, no other thread can enter a block protected by that monitor until the first thread exits the synchronized block.
	- Synchronization ensures that memory writes by a thread before or during a synchronized block are made visible in a predictable manner to other threads which synchronize on the same monitor.

- what is double-checked locking problem
	- The (infamous) double-checked locking idiom (also called the multithreaded singleton pattern) is a trick designed to support lazy initialization while avoiding the overhead of synchronization.


---


# Beyond Intrinsic Locks


## Problems with Intrinsic locks

- There is *no way* to interrupt a thread that’s blocked as a result of trying to acquire an intrinsic lock.
- There is *no way* to time out while trying to acquire an intrinsic lock.
- There’s exactly one way to acquire an intrinsic lock: a `synchronized` block.
	- This means that lock acquisition and release have to take place in the same method and have to be strictly nested.


## Reentrant Locks

[documentation](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/ReentrantLock.html)


`ReentrantLock` allows us to transcend above restrictions by providing explicit lock and unlock methods instead of using synchronized.


```java
Lock lock = new ReentrantLock();
lock.lock();
try {
	«use shared resources»
} finally {
	// good practice to ensure lock is always released
	lock.unlock();
}
```


`ReentrantLocks`:
- provide a `lockInterruptibly()` method to allow interrupting deadlocked threads
- provide a `tryLock()` method which takes a numeric value to allow this thread to time out in case a lock is not received.
	- while `tryLock` avoids infinite deadlock, that does not mean it's a good solution
	- Firstly, it doesn’t avoid deadlock—it simply provides a way to recover when it happens. 
	- Secondly, it’s susceptible to a phenomenon known as livelock - if all the threads time out at the same time, it’s quite possible for them to immediately deadlock again. Although the deadlock doesn’t last forever, no progress is made either.
	- This situation can be mitigated by having each thread use a different timeout value, for example, to minimize the chances that they will all time out simultaneously. 
	- But the bottom line is that timeouts are rarely a good solution—it’s far better to avoid deadlock in the first place.


### Hand-over-Hand locking

Hand-over-Hand locking allows us to lock only certain portions of a data structure (instead of the full structure), thus allowing other threads unfettered access to other potions.

![[Pasted image 20230910214731.png]]

Useful, for example, in concurrent Linked List implementation
- for insertion, we lock two nodes at one time, starting from left, and check if we can insert between those nodes
- For calculation size, we lock 1 node at a time and increment counter to ensure


### Condition Variables

Concurrent programming often involves waiting for something to happen.
Perhaps we need to wait for a queue to become nonempty before removing
an element from it. Or we need to wait for space to be available in a buffer
before adding something to it. This type of situation is what condition variables
are designed to address.

```java
ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();

lock.lock();

try {
	while (!«condition is true»)
	condition.await();
	«use shared resources»
} finally { lock.unlock(); }
```

A condition variable is associated with a lock, and a thread must hold that lock before being able to wait on the condition. 
- Once it holds the lock, it checks to see if the condition that it’s interested in is already `true`. 
- If it is `true`, then it continues with whatever it wants to do and unlocks the lock.
- If, however, the condition is `false`, it calls `await()`, which atomically unlocks the lock and blocks on the condition variable. 
	- An operation is atomic if, from the point of view of another thread, it appears to be a single operation that has either happened or not—it never appears to be halfway through.
- When another thread calls `signal()` or `signalAll()` to indicate that the condition might now be true, `await()` unblocks and automatically reacquires the lock. 
- An important point is that when `await()` returns, it only indicates that the condition might be true. 
	- This is why `await()` is called within a loop—we need to go back, recheck whether the condition is true, and potentially block on `await()` again if necessary.

here's a Dining philosopher implementation using condition variables

```java

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

import java.util.Random;

class Philosopher extends Thread {

  private boolean eating;
  private Philosopher left;
  private Philosopher right;
  private ReentrantLock table;
  private Condition condition;
  private Random random;
  private int thinkCount;

  public Philosopher(ReentrantLock table) {
    eating = false;
    this.table = table;
    condition = table.newCondition();
    random = new Random();
  }

  public void setLeft(Philosopher left) { this.left = left; }
  public void setRight(Philosopher right) { this.right = right; }

  public void run() {
    try {
      while (true) {
        think();
        eat();
      }
    } catch (InterruptedException e) {}
  }

  private void think() throws InterruptedException {
    table.lock();
    try {
      eating = false;
      // signal other philosopher that I am thinking so they may start eating
      left.condition.signal(); 
      right.condition.signal();
    } finally { table.unlock(); }
    ++thinkCount;
    if (thinkCount % 10 == 0)
      System.out.println("Philosopher " + this + " has thought " + thinkCount + " times");
    Thread.sleep(1000);
  }

  private void eat() throws InterruptedException {
    table.lock();
    try {
      while (left.eating || right.eating)
	      // wait till one of the neighbors has stopped eating
        condition.await();
      eating = true;
    } finally { table.unlock(); }
    Thread.sleep(1000);
  }
}
```



## Atomic Variables

Using an atomic variable instead of locks has a number of benefits. 
- First, it’s not possible to forget to acquire the lock when necessary. 
- Second, because no locks are involved, it’s impossible for an operation on an atomic variable to deadlock.
- Finally, atomic variables are the foundation of non-blocking, lock-free algorithms, which achieve synchronization without locks or blocking.

```java
public class Counting {
	public static void main(String[] args) throws InterruptedException {
	
		final AtomicInteger counter = new AtomicInteger();
		
		class CountingThread extends Thread {
			public void run() {
				for(int x = 0; x < 10000; ++x)
				counter.incrementAndGet();
			}
		}
	
		CountingThread t1 = new CountingThread();
		CountingThread t2 = new CountingThread();
		
		t1.start(); 
		t2.start();
		t1.join(); 
		t2.join();
		
		System.out.println(counter.get());
	}
}
```

`AtomicInteger`’s `incrementAndGet()` method is functionally equivalent to ++count (it
also supports `getAndIncrement`, which is equivalent to count++). Unlike ++count,
however, it’s atomic.


## Questions

- ReentrantLock supports a fairness parameter. What does it mean for a lock to be “fair”? Why might you choose to use a fair lock? Why might you not?

The constructor for `ReentrantLock`accepts an optional _fairness_ parameter. 
- When set `true`, under contention, locks favor granting access to the longest-waiting thread. 
- Otherwise this lock does not guarantee any particular access order. 
- Programs using fair locks accessed by many threads may display lower overall throughput (i.e., are slower; often much slower) than those using the default setting, but have smaller variances in times to obtain locks and guarantee lack of starvation. 
- Note however, that fairness of locks does not guarantee fairness of thread scheduling. Thus, one of many threads using a fair lock may obtain it multiple times in succession while other active threads are not progressing and not currently holding the lock.


- What is a `ReentrantReadWriteLock`?

Read here: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/ReentrantReadWriteLock.html



- What is spurious wake-up?

https://en.wikipedia.org/wiki/Spurious_wakeup

A **spurious wakeup** happens when a thread wakes up from waiting on a [condition variable](https://en.wikipedia.org/wiki/Condition_variable "Condition variable") that's been signaled, only to discover that the condition it was waiting for isn't satisfied. 
- It's called spurious because the thread has seemingly been awakened for no reason. 
- But spurious wakeup don't happen for no reason: they usually happen because, in between the time when the condition variable was signaled and when the waiting thread finally ran, another thread ran and changed the condition. 
- There was a [race condition](https://en.wikipedia.org/wiki/Race_condition "Race condition") between the threads, with the typical result that sometimes, the thread waking up on the condition variable runs first, winning the race, and sometimes it runs second, losing the race.


---


