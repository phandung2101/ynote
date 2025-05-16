# Đệ quy (Recursion) là gì?

Đệ quy là kỹ thuật giải quyết vấn đề bằng cách chia nhỏ vấn đề thành các vấn đề con tương tự, và một hàm tự gọi lại chính nó để xử lý các vấn đề con đó.

Ví dụ đơn giản về đệ quy - Tính giai thừa:

```Java
java
Copy
int factorial(int n) {
// Điều kiện dừng
    if (n <= 1) return 1;

// Công thức đệ quy
    return n * factorial(n-1);
}
```

# Cấu trúc của một hàm đệ quy:

1. Base case (Điều kiện dừng):
    - Trường hợp đơn giản nhất
    - Không cần gọi đệ quy
    - PHẢI có để tránh đệ quy vô hạn
2. Recursive case (Công thức đệ quy):
    - Chia vấn đề thành các vấn đề con
    - Gọi lại chính hàm đó
    - Kết hợp kết quả của các vấn đề con

# Ứng dụng của đệ quy:

1. Duyệt cấu trúc dữ liệu đệ quy:
    - Cây nhị phân
    - Đồ thị
    - File system
2. Các thuật toán chia để trị:
    - Quick Sort
    - Merge Sort
    - Binary Search
3. Các bài toán tổ hợp:
    - Sinh hoán vị
    - Sinh tập con
    - Bài toán N quân hậu

# Ưu điểm:

1. Code ngắn gọn, dễ đọc
2. Tự nhiên với các vấn đề có tính chất đệ quy
3. Dễ implement với các cấu trúc dữ liệu phức tạp
4. Dễ chứng minh tính đúng đắn

# Nhược điểm:

1. Tốn bộ nhớ stack:
    - Mỗi lời gọi đệ quy tạo stack frame mới
    - Có thể gây tràn stack với input lớn
2. Hiệu năng:
    - Overhead của việc gọi hàm
    - Có thể tính toán trùng lặp (như Fibonacci đệ quy)
3. Khó debug:
    - Call stack phức tạp
    - Khó theo dõi luồng thực thi

# Cách tối ưu đệ quy:

## Tail Recursion (Đệ quy đuôi):

```Java
java
Copy
// Đệ quy thông thường tính giai thừa
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);// Phải chờ factorial(n-1) để nhân với n
}

// Đệ quy đuôi
int factorialTail(int n, int result) {
    if (n <= 1) return result;
    return factorialTail(n-1, n * result);// Kết quả được tính toán trực tiếp
}
```

Ưu điểm:

- Compiler có thể tối ưu thành vòng lặp
- Không tốn thêm stack frame
- Hiệu quả hơn về bộ nhớ

## Memoization (Ghi nhớ):

```Java
java
Copy
class Fibonacci {
    private Map<Integer, Long> memo = new HashMap<>();

    public long fib(int n) {
// Kiểm tra trong bộ nhớ đệm
        if (memo.containsKey(n)) {
            return memo.get(n);
        }

// Tính toán và lưu vào bộ nhớ đệm
        long result;
        if (n <= 1) result = n;
        else result = fib(n-1) + fib(n-2);

        memo.put(n, result);
        return result;
    }
}
```

Ưu điểm:

- Tránh tính toán trùng lặp
- Giảm số lần gọi đệ quy
- Tăng tốc độ thực thi đáng kể

## Chuyển sang dạng lặp:

```Java
java
Copy
// Đệ quy
void traverseTree(Node root) {
    if (root == null) return;
    System.out.print(root.val);
    traverseTree(root.left);
    traverseTree(root.right);
}

// Chuyển thành dạng lặp với stack
void traverseTreeIterative(Node root) {
    Stack<Node> stack = new Stack<>();
    stack.push(root);

    while (!stack.isEmpty()) {
        Node current = stack.pop();
        if (current == null) continue;

        System.out.print(current.val);
        stack.push(current.right);
        stack.push(current.left);
    }
}
```

Ưu điểm:

- Kiểm soát được việc sử dụng bộ nhớ
- Tránh được stack overflow
- Dễ debug hơn

## Khi nào nên áp dụng

1. Tail Recursion:
    - Khi cần tối ưu bộ nhớ
    - Khi compiler hỗ trợ tối ưu tail recursion
2. Memoization:
    - Khi có nhiều tính toán trùng lặp
    - Khi cần tăng tốc độ thực thi
    - Đánh đổi bộ nhớ lấy tốc độ
3. Chuyển sang dạng lặp:
    - Khi lo ngại về stack overflow
    - Khi cần kiểm soát chi tiết quá trình thực thi
    - Khi cần debug dễ dàng hơn