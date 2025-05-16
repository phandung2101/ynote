# LinkedList Queue

LinkedList Queue là implementation phổ biến nhất của Queue interface trong Java.

### Cách sử dụng

```Java
import java.util.LinkedList;
import java.util.Queue;

public class LinkedListQueueExample {
    public static void main(String[] args) {
        Queue<String> queue = new LinkedList<>();
        
        // Thêm phần tử
        queue.offer("First");
        queue.offer("Second");
        queue.offer("Third");
        
        // In ra queue
        System.out.println("Queue: " + queue);
        
        // Lấy và xóa phần tử đầu
        String first = queue.poll();
        System.out.println("Phần tử được lấy ra: " + first);
        
        // Xem phần tử đầu không xóa
        String peek = queue.peek();
        System.out.println("Phần tử đầu queue: " + peek);
        
        // Kích thước queue
        System.out.println("Kích thước: " + queue.size());
    }
}
```

### Đặc điểm chính

- Thêm/xóa phần tử nhanh (O(1))
- Động và linh hoạt về kích thước
- Hỗ trợ phần tử null
- Phù hợp cho việc thường xuyên thêm/xóa

### Hạn chế

- Tốn bộ nhớ do lưu trữ tham chiếu
- Truy cập ngẫu nhiên chậm (O(n))
- Không hiệu quả về cache

## PriorityQueue

PriorityQueue là implementation đặc biệt cho phép sắp xếp phần tử theo độ ưu tiên.

### Cách sử dụng

```Java
import java.util.PriorityQueue;
import java.util.Comparator;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Tạo PriorityQueue với thứ tự tự nhiên
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        pq.offer(5);
        pq.offer(1);
        pq.offer(3);
        
        System.out.println("PriorityQueue tăng dần:");
        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
        
        // PriorityQueue với Comparator tùy chỉnh
        PriorityQueue<String> customPQ = new PriorityQueue<>(
            Comparator.comparing(String::length).reversed()
        );
        
        customPQ.offer("Java");
        customPQ.offer("Python");
        customPQ.offer("C++");
        
        System.out.println("\nPriorityQueue sắp xếp theo độ dài chuỗi giảm dần:");
        while (!customPQ.isEmpty()) {
            System.out.println(customPQ.poll());
        }
    }
}
```

### Đặc điểm chính

- Tự động sắp xếp theo độ ưu tiên
- Hỗ trợ tùy chỉnh quy tắc sắp xếp
- Hiệu quả cho việc lấy phần tử ưu tiên cao nhất
- Thích hợp cho thuật toán tham lam

### Hạn chế

- Thêm/xóa chậm hơn (O(log n))
- Không cho phép phần tử null
- Tốn tài nguyên cho việc duy trì heap

## ArrayDeque

ArrayDeque là implementation dựa trên mảng, cho phép thao tác ở cả hai đầu.

### Cách sử dụng

```Java
import java.util.ArrayDeque;

public class ArrayDequeExample {
    public static void main(String[] args) {
        ArrayDeque<String> deque = new ArrayDeque<>();
        
        // Thêm phần tử
        deque.addFirst("First");
        deque.addLast("Last");
        deque.add("Middle"); // Thêm vào cuối
        
        System.out.println("Deque: " + deque);
        
        // Thao tác với đầu deque
        String first = deque.pollFirst();
        System.out.println("Lấy phần tử đầu: " + first);
        
        // Thao tác với cuối deque
        String last = deque.pollLast();
        System.out.println("Lấy phần tử cuối: " + last);
        
        // Xem phần tử không xóa
        System.out.println("Phần tử đầu: " + deque.peekFirst());
        System.out.println("Phần tử cuối: " + deque.peekLast());
    }
}
```

### Đặc điểm chính

- Hiệu quả về mặt bộ nhớ
- Thao tác nhanh ở hai đầu (O(1))
- Có thể sử dụng như Stack hoặc Queue
- Hiệu suất cao

### Hạn chế

- Không hỗ trợ null
- Cần điều chỉnh kích thước khi mảng đầy
- Không hiệu quả khi thao tác ở giữa

## So sánh hiệu năng

|   |   |   |   |
|---|---|---|---|
|Operation|LinkedList|PriorityQueue|ArrayDeque|
|Thêm đầu|O(1)|O(log n)|O(1)|
|Thêm cuối|O(1)|O(log n)|O(1)|
|Xóa đầu|O(1)|O(log n)|O(1)|
|Xóa cuối|O(1)|O(log n)|O(1)|
|Truy cập|O(n)|O(n)|O(1)|
|Tìm kiếm|O(n)|O(n)|O(n)|

## Khi nào sử dụng?

### LinkedList Queue

- Producer-consumer pattern
- Khi cần thêm/xóa thường xuyên
- Không yêu cầu truy cập ngẫu nhiên
- Cần hỗ trợ null

### PriorityQueue

- Lập lịch tasks
- Thuật toán Dijkstra
- Thuật toán tham lam
- Xử lý theo độ ưu tiên

### ArrayDeque

- Cần thao tác nhanh ở hai đầu
- Thuật toán sliding window
- Yêu cầu hiệu suất cao
- Memory-sensitive applications

## Tổng kết

- **LinkedList**: Linh hoạt, thích hợp thêm/xóa nhiều
- **PriorityQueue**: Xử lý theo độ ưu tiên
- **ArrayDeque**: Hiệu suất cao, tiết kiệm bộ nhớ

Lựa chọn implementation phù hợp dựa trên:

- Yêu cầu hiệu năng
- Đặc thù ứng dụng
- Giới hạn bộ nhớ
- Tần suất các operation