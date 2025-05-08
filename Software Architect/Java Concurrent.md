# Thread basic

```java
final var thread = new Thread(() -> {
    for (int i = 10; i > 0; i--) {
        try {
            System.out.println("time is " + i);
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
});
thread.start();
try {
    thread.join();
} catch (InterruptedException e) {
    throw new RuntimeException(e);
}
System.out.println("boom");

```

Kết quả đoạn code sẽ chạy tuần tự khi sử dụng `thread.join()` để chờ thread kết thúc process

# ExecutorService

Có 3 loại:

- single thread pool executor
- cached thread pool executor
- fixed thread pool executor

```java
// CachedThreadPool
ExecutorService es = Executors.newCachedThreadPool();

// FixedThreadPool
int cpuCount = Runtime.getRuntime().availableProcessors();
ExecutorService es2 = Executors.newFixedThreadPool(cpuCount);

// SingleThreadPool
ExecutorService es3 = Executors.newSingleThreadExecutor();

```

# Sử dụng Future trong concurrent

## Future

```java
// SingleThreadPool
ExecutorService es = Executors.newSingleThreadExecutor();
final var futureIsHere = es.submit(() -> "what the heo");

try {
    System.out.println("Ket qua la: " +  futureIsHere.get(10, TimeUnit.SECONDS));
} catch (InterruptedException | ExecutionException | TimeoutException e) {
    throw new RuntimeException(e);
}
// Dùng để kết thúc process nếu không sẽ chạy mãi không buông
es.shutdown();

```

## Submitting task collections

```java
private static final ExecutorService es = Executors.newSingleThreadExecutor();
    private static final List<Callable<String>> callables = new ArrayList<>();

    public static void main(String[] args) {
        callables.add(() -> "A");
        callables.add(() -> "B");
        callables.add(() -> "C");
        callables.add(() -> "D");

//        invokeAny();
        invokeAll();
    }

    public static void invokeAny() {
        try {
            String result = es.invokeAny(callables);
            System.out.println("Ket qua la: " + result);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            es.shutdown();
        }
        System.out.println("Ket thuc");
    }

    public static void invokeAll() {
        try {
            List<Future<String>> results = es.invokeAll(callables);
            for (Future<String> result : results) {
                System.out.println("Ket qua la: " + result.get());
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            es.shutdown();
        }
        System.out.println("Ket thuc");
    }

```

# Data race

... (tự nghiên cứu)

# Scheduling tasks

... (tự nghiên cứu)

# Atomic Classes

... (tự nghiên cứu)

# Concurrent collections

## SkipListCollections

- Sorted by natural order
- ConcurrentSkipListSet <-> TreeSet
- ConcurrentSkipListMap <-> TreeMap

## CopyOnWriteCollections

```java
List<String> names = new CopyOnWriteArrayList<>();
names.add("Phan");
names.add("Viet");
names.add("Dung");

// Tạo ra snapshot khi khởi tạo 1 iterator. Array này sẽ không bao giờ thay đổi trong quá trình chạy iterator
// Tương tự cũng hoạt động với stream API
for (String name : names) {
    System.out.println(name);
    names.add(name);
}

// Với cách sử dụng index để duyệt thì không có tác dụng vì nó không phải là iterator
// Vòng lặp ở dưới sẽ chạy vô hạn
for (int i = 0; i < names.size(); i++) {
    System.out.println(names.get(i));
    names.add(names.get(i));
}

System.out.println(names); // [Phan, Viet, Dung, Phan, Viet, Dung]

```

## BlockingQueue

(tìm hiểu thêm...)

```java
BlockingQueue<String> queue = new LinkedBlockingQueue<>();
queue.offer("Red");    // head -> [Red] <- tail
queue.offer("Green");  // head -> [Red, Green] <- tail
queue.offer("Blue");   // head -> [Red, Green, Blue] <- tail
System.out.println(queue.poll()); // Red head -> [Green, Blue] <- tail
System.out.println(queue.peek()); // Green head -> [Green, Blue] <- tail
System.out.println(queue); // [Green, Blue]

try {
    // block is queue full
    queue.offer("White", 100 , TimeUnit.MILLISECONDS); // head -> [Green, Blue, White] <- tail
    queue.poll(200, TimeUnit.MILLISECONDS);               // head -> [Blue, White] <- tail
} catch (InterruptedException ex) {
    ex.printStackTrace();
}

```

## Synchronized Collections

Java có hỗ trợ việc tạo synchronized collections dựa trên conllections truyền thống nhưng sẽ có 1 số điểm yếu như:

- không thể modify khi sử dụng loop (throw exception)
- performance kém

```java
Map<String, String> capitalCities = new HashMap<>();

capitalCities.put("Ha Noi", "Viet Nam");
capitalCities.put("Sydney", "Australia");
capitalCities.put("Washington DC", "US");

Map<String, String> syncCapitalCities = Collections.synchronizedMap(capitalCities);
for (String key: syncCapitalCities.keySet()) {
    System.out.println(key + " is capital of " + syncCapitalCities.get(key));
    syncCapitalCities.remove(key); // throw ConcurrentModificationException
}

```

# Race Condition

- xảy ra khi 2 hoặc nhiều threads cố gắng truy cập resource cùng 1 thời điểm
- share resource này nên được truy cập 1 cách tuần tự
- Ví dụ:

```java
class BankAccount {
    private int balance = 50;

    public int getBalance() {
        return balance;
    }

    public void withdraw(int balance) {
        this.balance -= balance;
    }
}

public class RaceCondition implements Runnable {
    private BankAccount acct = new BankAccount();
    private Lock lock = new ReentrantLock();

    public static void main(String[] args) {
        Runnable r = new RaceCondition();
        Thread dung = new Thread(r);
        Thread chy = new Thread(r);
        dung.setName("Dung");
        chy.setName("Chy");
        dung.start();
        chy.start();
    }

    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            makeWithdrawSync(10);
            if (acct.getBalance() < 0) {
                System.out.println("Rút quá số tiền");
            }
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
            }        }
    }

    // Cách thông thường sử dụng sẽ gây ra lỗi khi xảy ra RaceCondition
    private void makeWithdrawWillBeError(final int amtToWithdraw) {
        if (acct.getBalance() >= amtToWithdraw) {
            System.out.println(Thread.currentThread().getName() + ". Trước khi rút: " + acct.getBalance());
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            acct.withdraw(amtToWithdraw);
            System.out.println(Thread.currentThread().getName() + ". Sau khi rút: " + acct.getBalance());
        } else {
            System.out.println(Thread.currentThread().getName() + ". Không thể rút. Số dư còn: " + acct.getBalance());
        }
    }
    /* Kết quả sai:
    Dung. Trước khi rút: 50    Chy. Trước khi rút: 50    Dung. Sau khi rút: 40    Chy. Sau khi rút: 30    Dung. Trước khi rút: 30    Chy. Trước khi rút: 30    Chy. Sau khi rút: 20    Dung. Sau khi rút: 10    Chy. Trước khi rút: 10    Dung. Trước khi rút: 10    Dung. Sau khi rút: 0    Rút quá số tiền    Chy. Sau khi rút: -10    Rút quá số tiền    Dung. Không thể rút. Số dư còn: -10    Rút quá số tiền    Chy. Không thể rút. Số dư còn: -10    Rút quá số tiền    Chy. Không thể rút. Số dư còn: -10    Rút quá số tiền    Dung. Không thể rút. Số dư còn: -10    Rút quá số tiền    */
    // Cách đúng thứ 1: Sử dụng synchronized ở method    private synchronized void makeWithdrawSync(final int amtToWithdraw) {
        if (acct.getBalance() >= amtToWithdraw) {
            System.out.println(Thread.currentThread().getName() + ". Trước khi rút: " + acct.getBalance());
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            acct.withdraw(amtToWithdraw);
            System.out.println(Thread.currentThread().getName() + ". Sau khi rút: " + acct.getBalance());
        } else {
            System.out.println(Thread.currentThread().getName() + ". Không thể rút. Số dư còn: " + acct.getBalance());
        }
    }
    /* Kết quả đúng:
    Dung. Trước khi rút: 50
    Dung. Sau khi rút: 40    Chy. Trước khi rút: 40    Chy. Sau khi rút: 30    Dung. Trước khi rút: 30    Dung. Sau khi rút: 20    Chy. Trước khi rút: 20    Chy. Sau khi rút: 10    Dung. Trước khi rút: 10    Dung. Sau khi rút: 0    Chy. Không thể rút. Số dư còn: 0    Dung. Không thể rút. Số dư còn: 0    Chy. Không thể rút. Số dư còn: 0    Chy. Không thể rút. Số dư còn: 0    Dung. Không thể rút. Số dư còn: 0    * */
    // Cách đúng thứ 2 sử dụng Lock    private void makeWithdrawWithLock(final int amtToWithdraw) {
        try {
            lock.lock();
            if (acct.getBalance() >= amtToWithdraw) {
                System.out.println(Thread.currentThread().getName() + ". Trước khi rút: " + acct.getBalance());
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                acct.withdraw(amtToWithdraw);
                System.out.println(Thread.currentThread().getName() + ". Sau khi rút: " + acct.getBalance());
            } else {
                System.out.println(Thread.currentThread().getName() + ". Không thể rút. Số dư còn: " + acct.getBalance());
            }
        } finally {
            lock.unlock();
        }

    }
}

```