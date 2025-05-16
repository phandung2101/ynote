# Khái niệm

- Linked List là một cấu trúc dữ liệu tuyến tính
- Bao gồm các phần tử (node) được liên kết với nhau thông qua con trỏ
- Mỗi node chứa dữ liệu và địa chỉ của node tiếp theo
- Không yêu cầu các phần tử được lưu trữ liên tiếp trong bộ nhớ

# Cấu trúc

## Node

- Data: Lưu trữ giá trị của phần tử
- Next pointer: Trỏ đến node tiếp theo
- (Optional) Previous pointer: Trỏ đến node trước đó (trong doubly linked list)

## Các thành phần chính

- Head: Node đầu tiên của danh sách
- Tail: Node cuối cùng của danh sách
- Null: Giá trị của pointer tại node cuối cùng

# Các loại Linked List

## Singly Linked List

- Mỗi node chỉ có một con trỏ trỏ tới node tiếp theo
- Chỉ có thể duyệt một chiều từ đầu đến cuối

## Doubly Linked List

- Mỗi node có hai con trỏ: next và previous
- Có thể duyệt hai chiều
- Chiếm nhiều bộ nhớ hơn

## Circular Linked List

- Node cuối cùng trỏ về node đầu tiên
- Có thể là singly hoặc doubly
- Thường dùng trong các ứng dụng xử lý vòng lặp

# Ưu và Nhược điểm

## Ưu điểm

- Kích thước động, không cần khai báo trước
- Thêm/xóa phần tử nhanh (O(1) nếu biết vị trí)
- Sử dụng bộ nhớ hiệu quả
- Dễ dàng triển khai các cấu trúc dữ liệu khác (Stack, Queue)

## Nhược điểm

- Không hỗ trợ truy cập ngẫu nhiên (O(n))
- Tốn thêm bộ nhớ cho con trỏ
- Cache locality kém hơn array
- Khó khăn trong việc duyệt ngược (với singly linked list)

# Cache Locality là gì?

Cache locality (hay locality of reference) là một khái niệm trong kiến trúc máy tính, liên quan đến việc tối ưu hóa hiệu suất bộ nhớ cache:

- Spatial Locality (Không gian): Khi CPU truy cập một địa chỉ bộ nhớ, nó cũng load các địa chỉ lân cận vào cache
    - Array có cache locality tốt vì các phần tử được lưu trữ liên tiếp trong bộ nhớ
    - Linked List có cache locality kém vì các node có thể nằm rải rác trong bộ nhớ
- Temporal Locality (Thời gian): Dữ liệu vừa được truy cập có khả năng cao sẽ được truy cập lại
    - Với Array, khi duyệt qua các phần tử, CPU chỉ cần load một lần vì dữ liệu đã nằm trong cache
    - Với Linked List, mỗi lần truy cập node tiếp theo có thể tạo ra cache miss vì node có thể nằm ở vùng nhớ khác

# Ứng dụng thực tế

## Quản lý playlist

- Mỗi node chứa thông tin bài hát
- Dễ dàng thêm/xóa bài hát
- Phù hợp cho chức năng next/previous

## Undo/Redo trong text editor

- Mỗi node lưu trữ một trạng thái
- Doubly linked list cho phép di chuyển qua lại giữa các trạng thái

## Browser history

- Mỗi node lưu URL và thông tin trang
- Điều hướng forward/backward

## Memory allocation

- Quản lý danh sách các khối bộ nhớ trống
- Thêm/xóa khối bộ nhớ linh hoạt

# Cài đặt cơ bản

```Java
java
Copy
public class Node<T> {
    private T data;
    private Node<T> next;

    public Node(T data) {
        this.data = data;
        this.next = null;
    }
}

public class LinkedList<T> {
    private Node<T> head;

    public LinkedList() {
        this.head = null;
    }

    public void append(T data) {
        Node<T> newNode = new Node<>(data);
        if (head == null) {
            head = newNode;
            return;
        }
        Node<T> current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
    }

    public void prepend(T data) {
        Node<T> newNode = new Node<>(data);
        newNode.next = head;
        head = newNode;
    }

    public void delete(T data) {
        if (head == null) return;

        if (head.data.equals(data)) {
            head = head.next;
            return;
        }

        Node<T> current = head;
        while (current.next != null && !current.next.data.equals(data)) {
            current = current.next;
        }

        if (current.next != null) {
            current.next = current.next.next;
        }
    }
}
```

# Best Practices

### Khi nên dùng

- Kích thước dữ liệu không xác định trước
- Cần thêm/xóa phần tử thường xuyên
- Không yêu cầu truy cập ngẫu nhiên
- Bộ nhớ phân mảnh

### Khi không nên dùng

- Cần truy cập ngẫu nhiên thường xuyên
- Kích thước dữ liệu cố định
- Yêu cầu cache locality cao
- Bộ nhớ hạn chế