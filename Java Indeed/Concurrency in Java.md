# what is java.util.concurrent ?
## Main components
- _Executor_
- _ExecutorService_
- _ScheduledExecutorService_
- _Future_
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

