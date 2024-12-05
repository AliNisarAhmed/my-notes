
## Synchronization

Whether an object needs to be thread-safe depends on whether it will be accessed from multiple threads. 
- This is a property of how the object is used in a program, not what it does. 
- Making an object thread-safe requires using synchronization to coordinate access to its mutable state; failing to do so could result in data corruption and other undesirable consequences.

**Rule**
Whenever more than one thread accesses a given state variable, and one of them might write to it, they all must coordinate their access to it using synchronization. 
- The primary mechanism for synchronization in Java is the `synchronized` keyword, which provides exclusive locking, but the term "synchronization" also includes the use of volatile variables,explicit locks,and atomic variables.


> If multiple threads access the same mutable state variable without appropriate synchronization, your program is broken.
> 
> There are three ways to fix it:
> - Don't share the state variable across threads
> - Make the state variable immutable
> - Use `synchorinzation` whenever accessing the state variable


## Thread Safety

A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synch or other coordination on the part of calling code.

If an object is thread-safe, no sequence of operations - calls to public methods or reads/writes of public fields - should be able to violate any of its **invariants** or post-conditions.

**A stateless object is always thread-safe** - like the example below:
```java
@ThreadSafe
public class StatelessFactorizer implements Servlet {
	public void service(ServletRequest req, ServletResponse resp) {
		BigInteger i = extractFromRequest(req);
		BigInteger[] factors = factor(i);
		encodeIntoResponse(resp, factors);
	}
}
```

## Atomicity

```java
@NotThreadSafe
public class UnsafeCountingFactorizer implements Servlet {
	private long count = 0;
	public long getCount() { return count; }
	public void service(ServletRequest req, ServletResponse resp) {
		BigInteger i = extractFromRequest(req);
		BigInteger[] factors = factor(i);
		++count;
		encodeIntoResponse(resp, factors);
	}
}
```

^12faef

The above code is not thread safe because `++count` operation is not a single "atomic" operation, i.e., another thread might update the value of `count` before it is updated/read
- this is called race condition

### Race conditions

A race condition occurs when the correctness of a computation depends on the relative timing of multiple threads by the runtime
- in other words, when getting the right answer relies on lucky timing

Most common types are
- **check-then-act**
	- example: initializing an expensive singleton object
- **read-modify-write**
	- `count` variable above

```java
// This is an example of check-then-act

@NotThreadSafe
public class LazyInitRace {
	private ExpensiveObject instance = null;
	public ExpensiveObject getInstance() {
		if (instance == null)
		instance = new ExpensiveObject();
		return instance;
	}
}
```

> A great analogy to remember race condition: You and your friend decide to meet at a starbuck on location X, however, when you reach there, you find there are two starbucks, A and B. What do you do now? if you stay at A your friend can be waiting at B, but if you go to B your friend might get to A while you are on your way


### Compound Actions

Operations A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread executes B, either all of B has executed or none of it has. An atomic operation is one that is atomic with respect to all operations, including itself, that operate on the same state.

To ensure thread safety, check-then-act operations (like lazy initialization) and read-modify-write operations (like increment) must always be atomic. These actions are called compound actions, these must be executed atomically in order to remain thread safe

`++count` operation here [[#^12faef]]  is a compound operation 

Using atomic variables fixes this issue:

```java
@ThreadSafe
public class CountingFactorizer implements Servlet {
	private final AtomicLong count = new AtomicLong(0);
	
	public long getCount() { return count.get(); }
	
	public void service(ServletRequest req, ServletResponse resp) {
		BigInteger i = extractFromRequest(req);
		BigInteger[] factors = factor(i);
		count.incrementAndGet();
		encodeIntoResponse(resp, factors);
	}
}
```

^c13116


## Locking

Contine the factor example, but now imagine we want to cache previously calculated factors:

```java
// not thread safe factoring with caching

@NotThreadSafe
public class UnsafeCachingFactorizer implements Servlet {
	private final AtomicReference<BigInteger> lastNumber = new AtomicReference<BigInteger>();
	
	private final AtomicReference<BigInteger[]> lastFactors = new AtomicReference<BigInteger[]>();
	
	public void service(ServletRequest req, ServletResponse resp) {
		BigInteger i = extractFromRequest(req);
		if (i.equals(lastNumber.get()))
			encodeIntoResponse(resp, lastFactors.get() );
		else {
			BigInteger[] factors = factor(i);
			lastNumber.set(i);
			lastFactors.set(factors);
			encodeIntoResponse(resp, factors);
		}
	}
}
```

^b3b53a

Unfortunately the above approach of using multiple Atomic variables does not work
- Even though the atomic references are individually thread-safe, the above code has race conditions that could make it produce the wrong answer.
- the invariant of the above class, that the factors cached in `lastFactors` equal the value cached in `lastNumber`, means when we update one, we must also update the other in the same "operation".
	- that's where locking helps


### Intrinsic Locking

`synchronized` block is built-in locking mechanism AKA intrinsic locking or monitor locks

Intrinsic locks are auto acquired before entering a `synchroinized` block, and auto released.
- `synchorinized` block is the only way actually to acquire intrinsic locks

Intrinsic locks act as `mutexes` (Mutual exclusion locks)
- which means that at most 1 thread may own the lock, while all other threads will have to wait

Synchronization makes it easy to fix factorization with caching [[#^b3b53a]]
```java
@ThreadSafe
public class SynchronizedFactorizer implements Servlet {
	@GuardedBy("this") private BigInteger lastNumber;
	@GuardedBy("this") private BigInteger[] lastFactors;
	
	public synchronized void service(ServletRequest req,
	ServletResponse resp) {
		BigInteger i = extractFromRequest(req);
		if (i.equals(lastNumber))
			encodeIntoResponse(resp, lastFactors);
		else {
			BigInteger[] factors = factor(i);
			lastNumber = i;
			lastFactors = factors;
			encodeIntoResponse(resp, factors);
		}
	}
}
```

^e66893

### Reentrancy

Reentrancy means that if a thread tries to acquire a lock *it already holds*, the request succeeds (as opposed to if another thread tries to acquire this lock_)
- i.e. a thread is allowed to re-acquire a lock it already holds

Reentrancy is implemented by associating with each lock an acquisition count and an owning thread.
- When the count is zero, the lock is considered unheld
- When a thread acquires a previously held lock, the JVM records the owner and sets the acquisition count to 1.
- If that same thread acquires the lock again, the count is incremented
- when the owning thread exits the `synchronized` block, the count is decremented
- When the count == 0, the lock is released

Without reentrant lock, the following code would deadlock in which a subclass overrides a `synchronized` method and then calls superclass method

```java
public class Widget {
	public synchronized void doSomething() {
	...
	}
}

public class LoggingWidget extends Widget {
	public synchronized void doSomething() {
		System.out.println(toString() + ": calling doSomething");
		super.doSomething();
	}
}
```

### Guarding State with Locks

Compound actions on shared state (such as incrementing a hit counter (read-modify-write) or lazy initialization (check-then-act)), must be made atomic to avoid race conditions.

Holding a lock for the entire duration of a compound action can make that compound action atomic.
- However, if `synchornization` is used to coordinate access to a variable, it is needed everywhere that variable is accessed.
- When using locks to coordinate access to a variable, the *same lock* must be used wherever that variable is accessed

>For each mutable state variable that may be accessed by more than one thread, all accesses to that variable must be performed with the same lock held. In this case, we say that the variable is guarded by that lock. 

e.g in this code block [[#^e66893]], the two fields are guarded by servlet object's intrinsic lock


Acquiring the lock associated with an object does not prevent other threads from accessing that object
- only acquiring the same lock is prevented
- as well as entering the `synch` blocks at the same time


A common  locking  convention is to encapsulate all mutable state within an object and to   protect it from concurrent access  by  synchronizing  any  code  path  that  accesses  mutable  state  using  the  object's intrinsic lock. This pattern is used by many thread-safe classes, such as Vector and other synchronized collection classes.  

For every invariant that involves more than 1 variable, all the variables involved in that invariant must be guarded by the same lock.

Merely `synch` every method, is not enough to render compound actions atmoic: e.g
```java
// not thread safem race condition still possible even if .contains and .add are themselves atomic

if (!vector.contains(element)) {
	vector.add(element)
}
```

### Liveness and Performance

Overuse of `synch` blocks can lead to performance issues. e.g. the class [[#^e66893]]

![[Pasted image 20240525112224.png]]

The following code fixes this, by separating code into 2 `synch` blocks

```java
@ThreadSafe
public class CachedFactorizer implements Servlet {
	@GuardedBy("this") private BigInteger lastNumber;
	@GuardedBy("this") private BigInteger[] lastFactors;
	@GuardedBy("this") private long hits;
	@GuardedBy("this") private long cacheHits;
	public synchronized long getHits() { return hits; }
	
	public synchronized double getCacheHitRatio() {
		return (double) cacheHits / (double) hits;
	}
	
	public void service(ServletRequest req, ServletResponse resp) {
		BigInteger i = extractFromRequest(req);
		BigInteger[] factors = null;
		synchronized (this) {
			++hits;
			if (i.equals(lastNumber)) {
				++cacheHits;
				factors = lastFactors.clone();
			}
		}
		if (factors == null) {
			factors = factor(i);
			synchronized (this) {
				lastNumber = i;
				lastFactors = factors.clone();
			}
		}
		encodeIntoResponse(resp, factors);
	}
}
```

The restructuring of `CachedFactorizer` provides a balance b/w simplicity (`synch` ing the entire method) vs concurrency (`synching` the shortest possible code paths)
- We don't want to break down `synch` blocks too far 
- the above code holds the lock when accessing state variables and for the duration of compound actions, but releases it before executing the potentially long-running factorization operation

> There is frequently a tension b/w simplicity and performance. When implementing a synch policy, resist the temptation to prematurely sacrifice simplicity (potentially compromising safety) for the sake of performance

> Avoid holding locks during length computations of operations at risk of not completing quickly such as network or console I/O