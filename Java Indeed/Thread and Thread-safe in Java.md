# Understanding Process vs Thread
## Process
- Process is **managed by OS**
- An independent program in execution
- Has its **own memory space**, file handles, system resource.
## Thread
- A **unit of execution** within a **process**
- All threads in same process **share the same memory**
- Lightweight compare to process, faster to create and manage
- Can communicate with other processes
- **Still independent in execution flow**
## Thread in Java
Java make working with Thread more easier. Using:
- Extend `Thread`
- Implement `Runnable`
- Lambda Expression of `Runnable`
- Implement `Callable` with `ExecutionService`
## Thread lifecycle in Java
![[thread_state.png]]
### New Thread
When new thread is created it in **new state**. The thread has not yet started.
### Runable State
A thread is **ready to run** is move to runable state. In this state, a thread might actually be running or it ready to run at any time. It's responsibilty of thread scheduler to give thread **time to run**. A multi-threaded program allocates fixed amount of time for each thread to run. After run thread pauses and gives up the CPU so the other threads can run.
### Blocked
The thread will be **blocked** when **trying to acquired a lock** but currently the lock is acquired by another thread. The thread will move from **blocked state** to **runnable state**  when it acquires the lock.
### Waiting State
The thread will be in waiting state when it call `wait()` method or `join()`. It will move to the runnable state when other thread will notify or that thread will be terminated
### Timed Waiting
When thread calls a method with **timeout parameters** until timeout is completed or until a notification is received. For example call `sleep()` or condition wait, it is moved to this state
### Terminate State
The thread terminates because of:
- because it complete normally
- or occured some error

```java
// Java program to demonstrate thread states 
// using a ticket booking scenario
class TicketBooking implements Runnable {
    @Override
    public void run() {
        
        try {
            
            // Timed waiting
            Thread.sleep(200); 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("State of bookingThread while mainThread is waiting: " +
                TicketSystem.mainThread.getState());

        try {
            
            // Another timed waiting
            Thread.sleep(100); 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class TicketSystem implements Runnable {
    public static Thread mainThread;
    public static TicketSystem ticketSystem;

    @Override
    public void run() {
        TicketBooking booking = new TicketBooking();
        Thread bookingThread = new Thread(booking);

        System.out.println("State after creating bookingThread: " + bookingThread.getState());

        bookingThread.start();
        System.out.println("State after starting bookingThread: " + bookingThread.getState());

        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("State after sleeping bookingThread: " + bookingThread.getState());

        try {
            
            // Moves mainThread to waiting state
            bookingThread.join(); 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("State after bookingThread finishes: " + bookingThread.getState());
    }

    public static void main(String[] args) {
        ticketSystem = new TicketSystem();
        mainThread = new Thread(ticketSystem);

        System.out.println("State after creating mainThread: " + mainThread.getState());

        mainThread.start();
        System.out.println("State after starting mainThread: " + mainThread.getState());
    }
}
```

![[thread_state_logs.png]]
# Thread-safe in Java
## What is thread-safe
An object or method can be safely called or used by multiple thread at the same time without causing errors or currupting data
## Problems
#### Data Corruption
```java
List<Integer> list = new ArrayList<>();

Runnable addTask = () -> {
    for (int i = 0; i < 1000; i++) {
        list.add(i);
    }
};

// Start multiple threads
Thread t1 = new Thread(addTask);
Thread t2 = new Thread(addTask);
t1.start();
t2.start();
```
What can happen:
- some element may be lost
- the size might not match number added
- internal array might grow inconsistently
#### ConcurrentModificationException
Occurs if one thread modifies the `ArrayList` while another is iterating over it.
```java
for (Integer i : list) {
    if (i == 5) {
        list.remove(i); // causes ConcurrentModificationException
    }
}
```
#### Unexpected behavior/Program Crashes
If two thread try to grow capacity of List this might:
- overwrite data
- leave the list in a corrupted state
- throw `ArrayIndexOutOfBoundsException` and `NullPointerException`
#### Inconsistent Views
Read old data or not real state of `ArrayList` for example
## Solutions
There are common solution like:
### Stateless implementation
Just simple don't use any state of implementation
```java
public class MathUtils {
    
    public static BigInteger factorial(int number) {
        BigInteger f = new BigInteger("1");
        for (int i = 2; i <= number; i++) {
            f = f.multiply(BigInteger.valueOf(i));
        }
        return f;
    }
}
```
### Immutable implementation
Using final variable
```java
public class MessageService {
    
    private final String message;

    public MessageService(String message) {
        this.message = message;
    }
    
    // standard getter
    
}
```

### Using thread-local field
Using private field that don't share any state with other thread.
```java
public class ThreadA extends Thread {
    
    private final List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    
    @Override
    public void run() {
        numbers.forEach(System.out::println);
    }
}
```
Similar we can create thread-local field by using `ThreadLocal`
```java
public class ThreadState {
    
    public static final ThreadLocal<StateHolder> statePerThread = new ThreadLocal<StateHolder>() {
        
        @Override
        protected StateHolder initialValue() {
            return new StateHolder("active");  
        }
    };

    public static StateHolder getState() {
        return statePerThread.get();
    }
}
```
Multiple thread access this `ThreadLocal` will access their copy version with init state is `initialValue()`.
### Using synchronized collections
This is wrapper layer that wrap collection as synchroinized collection. This mean **method can access by one thread at a time, while other thread are blocked by the first method**.
```java
Collection<Integer> syncCollection = Collections.synchronizedCollection(new ArrayList<>());
Thread thread1 = new Thread(() -> syncCollection.addAll(Arrays.asList(1, 2, 3, 4, 5, 6)));
Thread thread2 = new Thread(() -> syncCollection.addAll(Arrays.asList(7, 8, 9, 10, 11, 12)));
thread1.start();
thread2.start();
```
> Using synchronized has a penalty in performance, due to underlying logic of sync access. (just remember sync alway make thing slower than async but more controllable)
### Concurrent Collections
Alternatively to synchronized collections we can use concurrent collections to create thread-safe collections.
```java
Map<String,String> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("1", "one");
concurrentMap.put("2", "two");
concurrentMap.put("3", "three");
```
Unlike their synchornized counterparts, **concurrent collection achieve thread-safety by dividing their data into segments**. In a ConcurrentHashMap, several thread can acquire locks on different map segments, so multiple threads can access the Map at the same time.
>**Concurrent collections are** **much more performant than synchronized collections**. It’s worth mentioning that **synchronized and concurrent collections only make the collection itself thread-safe and not the contents.**
### Atomic Object
#### What is atomic 
**Atomic classes allow us to perform atomic operation, which are thread-safe, without using synchronization**.
Understanding that normal operation follow 3 step: read, modify, write. Very easy read step is reading old value. That make final value in correct.
Atomic operation make sure:
- indivisible: cannot be splitted
- no another thread can see uncomplete state
- auto sync
#### How Atomic class work in low level
Using **CPU-level instructions** (example like CAS - Compare-and-Swap) make sure incresement, decreasement, set value are thread-safe
```java
// demonstration
while (!atomicInt.compareAndSet(expectedValue, newValue)) {
    expectedValue = atomicInt.get(); // cập nhật giá trị mới nhất
    newValue = expectedValue + 1;
}
```
#### Java Atomic Class
Java provide **Atomic Class** such as `AtomicInteger`, `AtomicLong`, `AtomicBoolean` and `AtomicReference`. These class contain the following methods:
- set(int value): set to given value
- get(): return current value
- lazySet(int value): eventually sets to given value
- compareAndSet(int expect, int update): auto set to update if current value == expect value
- addAndGet(int delta): auto add given value to current value
- decrementAndGet(): decrease by one the current value

Usage example:
```java
// Using Atomic Variable
import java.util.concurrent.atomic.AtomicInteger;

class Counter 
{
    // Creating a variable of
    // class type AtomicInteger
    AtomicInteger count
        = new AtomicInteger();

    // Defining increment() method
    // to change value of
    // AtomicInteger variable
    public void increment() {
        count.incrementAndGet();
    }
}

public class TestCounter {
    public static void main(String[] args)
      		throws Exception
    {
        // Creating an instance of
        // Counter class
        Counter c = new Counter();

        // Creating an instance t1 of
        // Thread class
        Thread t1 = new Thread(new Runnable() 
        {
        	public void run() {
            	for(int i = 1; i <= 2000; i++) {
                	c.increment();
                }
            }
        });

        // Creating an instance t2
        // of Thread class
        Thread t2 = new Thread(new Runnable() 
        {
        	public void run() {
              	for (int i = 1; i <= 2000; i++) {
                  	c.increment();
                }
            }
        });

        // Calling start() method with t1 and t2
        t1.start();
        t2.start();

        // Calling join method with t1 and t2
        t1.join();
        t2.join();

        System.out.println(c.count);
    }
}
```


### Synchronized method
`synchronized` make method can be access by one thread at a moment.
```java
// Using Synchronization
class A 
{
    synchronized void sum(int n)
    {
        // Creating a thread instance
        Thread t = Thread.currentThread();
        for (int i = 1; i <= 5; i++) {
            System.out.println(t.getName() + " : " + (n + i));
        }
    }
}

// Class B extending thread class
class B extends Thread 
{
    // Creating an object of class A
    A a = new A();
    public void run()
    {

        // Calling sum() method
        a.sum(10);
    }
}

// Driver Class
class Geeks 
{
    public static void main(String[] args)
    {
        // Creating an object of class B
        B b = new B();

        // Initializing instance t1 of Thread
        // class with object of class B
        Thread t1 = new Thread(b);

        // Initializing instance t2 of Thread
        // class with object of class B
        Thread t2 = new Thread(b);

        // Initializing thread t1 with name
        //'Thread A'
        t1.setName("Thread A");

        // Initializing thread t2 with name
        //'Thread B'
        t2.setName("Thread B");

        // Starting thread instance t1 and t2
        t1.start();
        t2.start();
    }
}
```
### Synchronized statements
```java
public void incrementCounter() {
    // additional unsynced operations
    synchronized(this) {
        counter += 1; 
    }
}
```
### Using Volatile Keyword
#### Problem
**Example**:
```java
static class Task extends Thread {
	boolean running = true; // Not volatile

	public void run() {
		while (running) {
			// Simulate some work
		}
		System.out.println("Stopped!");
	}

	public void stopRunning() {
		running = false;
	}
}

public static void main(String[] args) throws InterruptedException {
	Task task = new Task();
	task.start();

	Thread.sleep(1000); // Let the thread run a bit
	task.stopRunning(); // Try to stop the thread

	task.join(); // Wait for thread to finish (might hang!)
}
```
In case above app never stop because:
- when running = true -> thread might cache value of running from main memory in the thread's local memory
- so when update running = true in main memory -> thread still using cached value
=> **This is called a visibility problem****
#### How `volatile` affect to memory save?
- One thing must be clear that `volatile` don't change what memory variable save
- It still the same but it's like said with thread that *"Hey bro, you must read and write this var in memory not the cache"*
#### Solution
Add `volatile` keyword to `running` like this
```java
...
volatile boolean running = true;
...
```
This will do:
- **Always read `running` from main memory - do not cache it**
- **Alway write `running` to main memory - so other thread can see it**
#### Real-world solution
In programming we might consider many thing more:
1. `volatile` only for **visibility problem** but not for **thread-safe**
2. We could use `Atomic*` class or `synchronized` 

### Reentrant Locks
**With intrinsic locks, the lock acquisition model is rather rigid**. Like one acquire lock and release lock for another thread to acquire lock. No undelying mechanism to give piority for longest thread waiting. 
`ReentrantLock` do exactly what we said above. **preventing queued threads from suffering some types of resource starvation**
```java
public class ReentrantLockCounter {

    private int counter;
    private final ReentrantLock reLock = new ReentrantLock(true);
    
    public void incrementCounter() {
        reLock.lock();
        try {
            counter += 1;
        } finally {
            reLock.unlock();
        }
    }
    
    // standard constructors / getter
    
}
```
### Read/Write Locks
It work like when no write multiple can read it. When one write no read can happen.
```java
public class ReentrantReadWriteLockCounter {
    
    private int counter;
    private final ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();
    
    public void incrementCounter() {
        writeLock.lock();
        try {
            counter += 1;
        } finally {
            writeLock.unlock();
        }
    }
    
    public int getCounter() {
        readLock.lock();
        try {
            return counter;
        } finally {
            readLock.unlock();
        }
    }

   // standard constructors
}
```