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
# Concurrent vs Parallel

## Concurrent
Is doing same time with flow. Like we have two person but only 1 knife for cutting job. So they will need to share their knife but I work very smooth without blocking like one waiting and one cooking another without give up a knife.
## Parallel
Same as concurrent but now we have 2 knife (real hardware like 2 CPU)

# Data race
Data race happen when 2 concurrent process access the same shared resource.
Example: withdraw same balance in 2 ATM machine. If our balance only have 50 but we do 2 process withdraw 50 if not handle data race well, can happen that withdraw total 100
**Defination**:
- more than 2 thread/process access same shared resource
- at least 1 doing update shared resource
## How to handle Data race
Most simple is use rule like only 1 can access at a time. The code use for read/write shared resource is called **critical section/critical region**
How to implement **critical section**?
## Mutual exclusion
The answer is using **Mutex (Mutual exclusion)**. These are 2 atomic process happen:
- Acquire lock process
- Release lock process
Example:
- thread A acquire lock success
- thread B acquire lock (will be block until lock released)
- thread A release lock (must do or this process will block forever)
- thread B acquire lock success
# Deadlock
Deadlock happen when **multiple-thread** is blocked because each process holding a resource and waiting for another resource acquired by some process.
Example:
- thread 1 acquire resource A
- thread 2 acquire resource B
- thread 1 trying acquire resource B to continue processing but blocked because thread 2 is acquire it.
- thread 2 trying acquire resource A to continue processing but blocked because thread 1 is acquire it.
=> this make complete blocked process and this call **deadlock**
# Abandoned Lock
Abandoned lock happen when process forgot to release after process success.
```java
doSomeThing();
// if success everything oke but what if exception happen
releaseLock();
```
Best practice will be:
```java
try {
	doSomething();
} finally {
	releaseLock();
}
```
# Thread Starvation
In best scenario, if thread A and thread B will be access resource alternatively. But in real world `Schedular` control all process and decide what process cound access resource. So what if thread B cannot piority access resource -> this call thread starvation
# Live Lock
A stituation in concurrent computing when two or more threads continous change thier state in response to each other, but no one make process toward.
Example: These are A and B trying to get into a gate that only fit one person. But they are waiting other to get in first -> make thing not productivity.
# Race condition
Race condition is different between Data race. Data race happen when they access the same shared resource. But race condition is about correctness depend on timing or order of execution. The result depend on order of execution process
This example is about race condition and data race happen:
```java
Thread A: if (queue.notEmpty()) { lock(); item = queue.dequeue(); unlock(); }
Thread B: if (queue.notEmpty()) { lock(); item = queue.dequeue(); unlock(); }
```
This one is only race condition happen but not data race because no shared resource
```java
public class RaceCondition {
    public static void main(String[] args) throws InterruptedException {
        var firstThread = new Thread(() -> IntStream.range(0, 1000)
                .forEach(i -> System.out.println("First thread " + i)));
        var secondThread = new Thread(() -> IntStream.range(0, 1000)
                .forEach(i -> System.out.println("Second thread" + i)));
        firstThread.start();
        secondThread.start();
        firstThread.join();
        secondThread.join();
    }
}
```
Each time you run this will have different result based on ordering process.
> I think race condition is bigger field than data racing because it relate too many thing not only coding. Like data race is race condition but not oposite.
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
# Concurrency package in Java
## Main components
- _Executor_
- _ExecutorService_
- _ScheduledExecutorService_
- _Future_
- _CompletableFuture_
- _CountDownLatch
- _CyclicBarrier_
- _Semaphore_
- _ThreadFactory_
- _BlockingQueue_
- _DelayQueue_
- _Locks_
- _Phaser_
## Executor 
Is a set of interface the represent an object whose implementation executes task. It depend on implement whether the task should be run in a new thread or in current thread. We can decouple the task execution flow from actual task execution mechanism by using this interface.
**The executor doesn't require task execution to be asynchronous**. The simplest of all execution interface.
```java
public class Invoker implements Executor {
   @Override
   public void execute(Runnable r) {
       r.run();
   }
}
```
And execute like this:
```java
public void execute() {
   Executor exe = new Invoker();
   exe.execute( () -> {
       // task to be performed
   });
}
```
## ExecutorService
Is interface that focus the underlying implementation to implement `execute()` method. It extend `Executor` interface and add series of method that execute threads that return the value. The methods to shut the threadpool as well as ability to implement for the result of execution of the task.
We create `Runable` as a target
```java
public class Task implements Runnable {
   @Override
   public void run() {
       // task details
   }
}
```
Now we create ExecutorService with specific thread-pool size
```java
// 20 is the thread pool size
ExecutorService exec = Executors.newFixedThreadPool(20);
// for single thread
ExecutorService exec = Executors.newSingleThreadExecuter(threadfactory)
```
After create now we can submit the task
```java
public void execute() {
   executor.submit(new Task());
}
// or we can also use lambda expression
executor.submit(() -> {
   new Task();
});
```
Two **out-of-the-box** termination methods are listed below:
1. shutdown(): wait until all the submitted tasks get finished
2. shutdownNow(): immediately terminates all pending/executing tasks
One more method is `awaitTermination(long timeout, TimeUnit unit)` do block the current thread util one of this happen:
- All submitted task complete after calling shutdown()
- The timeout expires
- The current thread is interrupted
Understand it running like this
```java
ExecutorService exec = Executors.newFixedThreadPool(20);
exec.submit(() -> new Task(1))
exec.submit(() -> new Task(2))

exec.shutdown() // this block for not allow new task to be submit
exec.awaitTermination(50, TimeUnit.SECOND)
```
## ScheduledExecutorService
It is similar to ExecutorService. Only different that they can perform task periodically. Both `Runable` and `Callabe` function is used to define the task.
```java
public void execute() {
   ScheduledExecutorService execServ
     = Executors.newSingleThreadScheduledExecutor();

   Future<String> future = execServ.schedule(() -> {
       // ..
       return "Hello world";
   }, 1, TimeUnit.SECONDS);

   ScheduledFuture<?> scheduledFuture = execServ.schedule(() -> {
       // ..
   }, 1, TimeUnit.SECONDS);

   executorService.shutdown();
}
```
`schedule` method will run task after a time.
But we can use:
- **scheduleAtFixedRate( Runnable command, long initialDelay, long period, TimeUnit unit)**: this method run first one after init delay and repeat with fixed time without duration of task completion.
- **scheduleWithFixedDelay( Runnable command, long initialDelay, long delay, TimeUnit unit)**: this method run first one after init delay and repeat with delay after previous task complete.
## Future
It represents the result of an **asynchronous operation**. The methods in it check if the asynchronous operation is complete or not, get the complete result.
```java
public void invoke() {
   ExecutorService executorService = Executors.newFixedThreadPool(20);

   Future<String> future = executorService.submit(() -> {
       // ...
       Thread.sleep(10000l);
       return "Hello";
   });
}
```
The code to check if the result of the future is ready or not and fetchs the data when the computation are done.
```java
if (future.isDone() && !future.isCancelled()) {
   try {
       str = future.get();
   } catch (InterruptedException | ExecutionException e) {
       e.printStackTrace();
   }
}
```
**Timeout specification** for a given operation. If the time take is more than this time, then **TimeoutException** will thrown.
```java
try {
   future.get(20, TimeUnit.SECONDS);
} catch (InterruptedException | ExecutionException | TimeoutException e) {
   e.printStackTrace();
}
```
## CompletableFuture
It is newer class that doesn't need ExecutorService to submit task to run a task async.
```java
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(() -> "Hello");

CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> "Hello");
```
Cleary better right. But one another different is:

| Feature  | `Future`                                           | `CompletableFuture`                                             |
| -------- | -------------------------------------------------- | --------------------------------------------------------------- |
| Blocking | `get()` block the thread until the result is ready | Can use non-blocking methods like `thenApply()`, `thenAccept()` |
| Chaining | Not support                                        | Support fluent chainning                                        |
```java
// CompletableFuture: Non-blocking chain
CompletableFuture.supplyAsync(() -> "Hello")
                 .thenApply(s -> s + " World")
                 .thenAccept(System.out::println);

```
## CountDownLatch
`CountDownLatch` is a counter that init with a number, amount of threads that main thread need to wait for all be done and after that it release main thread.
```java
// Java Program to demonstrate how 
// to use CountDownLatch, Its used 
// when a thread needs to wait for other 
// threads before starting its work.
import java.util.concurrent.CountDownLatch;

public class CountDownLatchDemo
{
    public static void main(String args[]) 
                   throws InterruptedException
    {
        // Let us create task that is going to 
        // wait for four threads before it starts
        CountDownLatch latch = new CountDownLatch(4);

        // Let us create four worker 
        // threads and start them.
        Worker first = new Worker(1000, latch, 
                                  "WORKER-1");
        Worker second = new Worker(2000, latch, 
                                  "WORKER-2");
        Worker third = new Worker(3000, latch, 
                                  "WORKER-3");
        Worker fourth = new Worker(4000, latch, 
                                  "WORKER-4");
        first.start();
        second.start();
        third.start();
        fourth.start();

        // The main task waits for four threads
        latch.await();

        // Main thread has started
        System.out.println(Thread.currentThread().getName() +
                           " has finished");
    }
}

// A class to represent threads for which
// the main thread waits.
class Worker extends Thread
{
    private int delay;
    private CountDownLatch latch;

    public Worker(int delay, CountDownLatch latch,
                                    String name)
    {
        super(name);
        this.delay = delay;
        this.latch = latch;
    }

    @Override
    public void run()
    {
        try
        {
            Thread.sleep(delay);
            latch.countDown();
            System.out.println(Thread.currentThread().getName()
                            + " finished");
        }
        catch (InterruptedException e)
        {
            e.printStackTrace();
        }
    }
}
```
## CyclicBarrier
Same as `CountDownLatch` except that we can reuse it. It allows us to reuse because when it release we can use it again with same number.

We need to add `await()` after all number of await release. Main thread release.
```java
public class Task implements Runnable {

   private CyclicBarrier barrier;

   public Task(CyclicBarrier barrier) {
       this.barrier = barrier;
   }

   @Override
   public void run() {
       try {
           LOG.info(Thread.currentThread().getName() +
             " is waiting");
           barrier.await();
           LOG.info(Thread.currentThread().getName() +
             " is released");
       } catch (InterruptedException | BrokenBarrierException e) {
           e.printStackTrace();
       }
   }
}
```
Execute task and check if cyclicBarrier is broke
```java
public void start() {

   CyclicBarrier cyclicBarrier = new CyclicBarrier(3, () -> {
       // ..
       LOG.info("All previous tasks completed");
   });

   Thread t11 = new Thread(new Task(cyclicBarrier), "T11");
   Thread t12 = new Thread(new Task(cyclicBarrier), "T12");
   Thread t13 = new Thread(new Task(cyclicBarrier), "T13");

   if (!cyclicBarrier.isBroken()) {
       t11.start();
       t12.start();
       t13.start();
   }
}
```
## Phaser
`Phaser` is barrier style asynchronization tool that allows multiple threads to wait for each other, in phases.
Imagine a game with multiple rounds, and in each round:
- All players (threads) must complete their turn
- Once all players are done, the next round (phase) begins
- New players can join mid-game; others can drop out
```java
Phaser phaser = new Phaser(3); // Register 3 parties

Runnable task = () -> {
    String name = Thread.currentThread().getName();

    System.out.println(name + " arrived at phase " + phaser.getPhase());
    phaser.arriveAndAwaitAdvance(); // wait for others

    System.out.println(name + " completed phase 0");

    phaser.arriveAndAwaitAdvance(); // phase 1
    System.out.println(name + " completed phase 1");
};

for (int i = 0; i < 3; i++) {
    new Thread(task, "Thread-" + i).start();
}
```
> Should be read more about it.
## Semaphore
That is like how many thread can access resource at the same time. Example we have a parking spot that only have 10 slot, when full no one can access anymore until another one leaving.
```java
Semaphore semaphore = new Semaphore(2); // only 2 permits

Runnable task = () -> {
    try {
        System.out.println(Thread.currentThread().getName() + " waiting to acquire...");
        semaphore.acquire(); // blocks if no permit
        System.out.println(Thread.currentThread().getName() + " acquired!");

        Thread.sleep(2000); // simulate work

    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        System.out.println(Thread.currentThread().getName() + " releasing...");
        semaphore.release();
    }
};

// Run 5 threads
for (int i = 0; i < 5; i++) {
    new Thread(task).start();
}

```
## ThreadFactory
It act as a thread pool which creates a new thread on demand. `ThreadFactory` can be defined as:
```java
public class GFGThreadFactory implements ThreadFactory {
   private int threadId;
   private String name;

   public GFGThreadFactory(String name) {
       threadId = 1;
       this.name = name;
   }

   @Override
   public Thread newThread(Runnable r) {
       Thread t = new Thread(r, name + "-Thread_" + threadId);
       LOG.info("created new thread with id : " + threadId +
           " and name : " + t.getName());
       threadId++;
       return t;
   }
}
```
## BlockingQueue
`BlockingQueue` is an interface that represents a `thread-safe`queue. It supports **waiting (blocking)** operations when:
- `put()` an element and the queue is full
- `take()` an element and the queue is empty
## DelayQueue
`DelayQueue` is a queue of elements with delay. That mean:
- You't can take item from the queue **until its delay has expired**
- It is **unbounded** and based on `PriorityQueue`
## Lock
It's a utility for **block other thread** to accessing a certain segment of code. The different between **lock** and **asynchronous block** that we have `Lock` APIs `lock()` and `unlock()`.

## Runable
It's an interface for run a task without return value and cannot throw checked exception
```java
@Override  
public void run()  {  
    System.out.printf("Process log number #%d%n", logNum);  
    try {  
        Thread.sleep(Math.round(random() * 1000));  
    } catch (InterruptedException e) { //you must handle exception  
        throw new RuntimeException(e);  
    }  
    cdl.countDown();  
}
```
## Callable
It's an interface like `Runable` but it have return value and can throw checked exception

```java
@Override  
public String call() throws Exception {  
    System.out.printf("Process log number #%d%n", logNum);  
    Thread.sleep(Math.round(random() * 1000));  
    cdl.countDown();  
    return "";  
}
```



