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
# `Volatile` in Java
## Problem
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
## How `volatile` affect to memory save?
- One thing must be clear that `volatile` don't change what memory variable save
- It still the same but it's like said with thread that *"Hey bro, you must read and write this var in memory not the cache"*
## Solution
Add `volatile` keyword to `running` like this
```java
...
volatile boolean running = true;
...
```
This will do:
- **Always read `running` from main memory - do not cache it**
- **Alway write `running` to main memory - so other thread can see it**
## Real-world solution
In programming we might consider many thing more:
1. `volatile` only for **visibility problem** but not for **thread-safe**
2. We could use `Atomic*` class or `synchronized` 
# `Synchronized` in Java
