## 1. **Web Scraper with Thread Pool**
**Problem**: You want to scrape data from 100 websites in parallel, but only allow 10 connections at a time.

**Concurrency Tools**:
- `ExecutorService` with fixed thread pool (`Executors.newFixedThreadPool(10)`)
- `Future` to track each result
- `invokeAll()` to wait for all
**Example:**
```java
static class WebScraper implements Callable<String> {  
    final String web;  
  
    WebScraper(final String web) {  
        this.web = web;  
    }  
  
    @Override  
    public String call() {  
        try {  
            System.out.println("Process crawling " + this.web);  
            Thread.sleep(Math.round(random() * 10000));  
            return this.web + " done!";  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}  
  
public static void main(String[] args) throws InterruptedException, ExecutionException {  
    List<String> webs = new ArrayList<>();  
    for (int i = 1; i <= 100; i++) {  
        webs.add("yezweb " + i);  
    }  
    ExecutorService exec = Executors.newFixedThreadPool(10);  
    var webScrapers = webs.stream().map(WebScraper::new).toList();  
    List<Future<String>> results = exec.invokeAll(webScrapers);  
  
    for (var result : results) {  
        System.out.println(result.get());  
    }  
  
    exec.shutdown();  
  
}
```
## 2. **Batch File Processor**
**Problem**: Process 1000 log files in parallel. After all are done, generate a summary report.

**Concurrency Tools**:
- `ExecutorService`
- `CountDownLatch` to wait for all threads to finish

**Example:**
```java
record LogProcessor(int logNum, CountDownLatch cdl) implements Callable<Long> {  
  
    @Override  
    public Long call() throws Exception {  
        System.out.printf("Process log number #%d%n", logNum);  
        final long time = Math.round(random() * 1000);  
        Thread.sleep(time);  
        cdl.countDown();  
        return time;  
    }  
}  
  
public static void main(String[] args) throws InterruptedException, ExecutionException {  
    CountDownLatch cdl = new CountDownLatch(1000);  
    ExecutorService exec = Executors.newFixedThreadPool(10);  
    List<Future<Long>> results = new ArrayList<>();  
    for (int i = 1; i <= 1000; i++) {  
        results.add(exec.submit(new LogProcessor(i, cdl)));  
    }  
  
    System.out.println("Shutdown exec");  
    exec.shutdown();  
  
    System.out.println("CDL waiting");  
    cdl.await();  
  
    System.out.println("Proceed 1000 log file");  
  
    long sum = 0;  
    for (var result : results) {  
        sum += result.get();  
    }  
  
    System.out.printf("Log avg time proceed is %dms%n", sum / 1000);  
}
```
## 3. **Simultaneous Game Start**
**Problem**: 4 players need to be ready before the game starts. The game should only start when all 4 are ready.

**Concurrency Tools**:
- `CyclicBarrier` (all threads wait at the barrier, then proceed together)

**Example:**
```java
static class Player implements Runnable {  
    private final int id;  
    private final CyclicBarrier barrier;  
  
    Player(int id, CyclicBarrier barrier) {  
        this.id = id;  
        this.barrier = barrier;  
    }  
  
    @Override  
    public void run() {  
        try {  
            System.out.printf("Player %d is getting ready...\n", id);  
            Thread.sleep((long) (Math.random() * 10000));  
            System.out.printf("Player %d is ready!\n", id);  
            barrier.await();  
            System.out.printf("Player %d starts playing!\n", id);  
        } catch (InterruptedException | BrokenBarrierException e) {  
            e.printStackTrace();  
        }  
    }  
}  
  
public static void main(String[] args) {  
    int numPlayers = 4;  
    CyclicBarrier barrier = new CyclicBarrier(numPlayers, () ->  
            System.out.println("All players are ready. Game starts NOW!\n")  
    );  
  
    ExecutorService exec = Executors.newFixedThreadPool(numPlayers);  
  
    for (int i = 1; i <= numPlayers; i++) {  
        exec.submit(new Player(i, barrier));  
    }  
  
    exec.shutdown();  
}
```
## 4. **Rate-Limited API Access**
**Problem**: Only 5 requests per second can be sent to an external API.

**Concurrency Tools**:
- `Semaphore` to limit concurrent access
- Scheduled executor for pacing

**Example:**
```java
// Allow 5 requests per second  
private static final int MAX_REQUESTS_PER_SECOND = 5;  
private static final Semaphore semaphore = new Semaphore(MAX_REQUESTS_PER_SECOND);  
  
public static void main(String[] args) {  
  
    ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);  
    /* you can decrease number of thread pool to make maximum concurrency */  
    ExecutorService workers = Executors.newFixedThreadPool(1000);   
  
    // Reset the permits every second  
    scheduler.scheduleAtFixedRate(() -> {  
        int permitsToAdd = MAX_REQUESTS_PER_SECOND - semaphore.availablePermits();  
        if (permitsToAdd > 0) {  
            semaphore.release(permitsToAdd);  
            System.out.println(">> Resetting permits to " + MAX_REQUESTS_PER_SECOND);  
        }  
    }, 0, 1, TimeUnit.SECONDS);  
  
    // Simulate 100 API requests  
    for (int i = 1; i <= 100; i++) {  
        int requestId = i;  
        workers.submit(() -> {  
            try {  
                semaphore.acquire(); // Wait if no permits  
                callExternalApi(requestId);  
            } catch (InterruptedException e) {  
                Thread.currentThread().interrupt();  
            }  
        });  
    }  
  
    workers.shutdown();  
}  
  
private static void callExternalApi(int requestId) {  
    System.out.printf("Request #%d sent at %d ms\n", requestId, System.currentTimeMillis() % 100000);  
    try {  
        Thread.sleep(Math.round(Math.random()*10000)); // Simulate API call delay  
    } catch (InterruptedException e) {  
        Thread.currentThread().interrupt();  
    }  
}
```
## 5. **Producer-Consumer Queue**
**Problem**: One or more producer threads write messages to a queue, while multiple consumer threads process them.

**Concurrency Tools**:
- `BlockingQueue` (`ArrayBlockingQueue`, `LinkedBlockingQueue`)
- Thread-safe and avoids manual synchronization

**Example:**
```java
public static void main(String[] args) {  
    int PRODUCER_NUMBER = 5;  
    int CONSUMER_NUMBER = 10;  
    int NUMBER_MESSAGE_PER_PRODUCER = 20;  
    int MAX_QUEUE_CAP = 20;  
    ExecutorService exec = Executors.newFixedThreadPool(PRODUCER_NUMBER + CONSUMER_NUMBER);  
    BlockingQueue<String> queue = new LinkedBlockingQueue<>(MAX_QUEUE_CAP);  
    for (int i = 0; i < PRODUCER_NUMBER; i++) {  
        final int producerNumber = i + 1;  
        exec.submit(() -> {  
            for (int j = 0; j < NUMBER_MESSAGE_PER_PRODUCER; j++) {  
                try {  
                    Thread.sleep(Math.round(Math.random() * 1000)); // simulate delay to queue server  
                    queue.put("#%d.%d".formatted(producerNumber, j));  
                    System.out.println("Producer-" + producerNumber + " -> sent message number #" + j);  
                } catch (InterruptedException e) {  
                    throw new RuntimeException(e);  
                }  
            }  
            System.out.println("Producer-" + producerNumber + " -> disconnected");  
        });  
    }  
  
    for (int i = 0; i < CONSUMER_NUMBER; i++) {  
        final int consumerNumber = i + 1;  
        exec.submit(() -> {  
            while (true) {  
                try {  
                    String message = queue.take();  
                    Thread.sleep(Math.round(Math.random() * 2000));  // simulate proceed time consume
                    System.out.println("Consumer-" + consumerNumber + " -> proceed message number " + message);  
                } catch (InterruptedException e) {  
                    throw new RuntimeException(e);  
                }  
            }  
        });  
    }  
  
    exec.shutdown();  
  
}
```
## 6. **Async Image Downloader with Progress Update**
**Problem**: Download 50 images asynchronously. Update progress bar as each one finishes.

**Concurrency Tools**:
- `CompletableFuture.supplyAsync()`
- `thenAccept()` or `thenRun()` to update UI/progress
- `CompletableFuture.allOf()` to wait for all downloads
## 7. **Delayed Task Execution**
**Problem**: You want to delay a task (e.g., send a reminder email) by 10 seconds or run tasks at fixed intervals.

**Concurrency Tools**:
- `ScheduledExecutorService`
- `schedule()` or `scheduleAtFixedRate()`
## 8. **Parallel Matrix Multiplication**
**Problem**: Speed up matrix multiplication using parallel tasks.

**Concurrency Tools**:
- `ForkJoinPool` for divide-and-conquer parallelism
- `RecursiveTask` or `CompletableFuture`
## 9. **Multi-stage Pipeline**
**Problem**: Data flows through multiple stages (e.g., Parse → Filter → Transform → Save), with each stage running in parallel.

**Concurrency Tools**:
- `LinkedBlockingQueue` between stages
- Producer-Consumer pattern between pipeline steps
## 10. **Handling Timeouts in Async Tasks**
**Problem**: Perform a slow operation (like DB query) but fail fast if it doesn’t respond within 5 seconds.

**Concurrency Tools**:
- `Future.get(timeout, TimeUnit)`
- or `CompletableFuture.orTimeout(...)`