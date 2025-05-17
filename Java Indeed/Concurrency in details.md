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
