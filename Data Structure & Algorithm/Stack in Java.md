# Implement Stack trong Java

## 1. Stack class có sẵn (extends Vector)

```Java
Stack<Integer> stack = new Stack<>();
```

**Ưu điểm:**

- Đơn giản, dễ sử dụng
- Có sẵn các phương thức cần thiết

**Nhược điểm:**

- Kế thừa từ Vector nên thread-safe, performance không tốt bằng các lựa chọn khác
- Vector là legacy class, không được khuyến khích sử dụng

## 2. ArrayDeque

```Java
Deque<Integer> stack = new ArrayDeque<>();
// push() = addFirst()
// pop() = removeFirst()
```

**Ưu điểm:**

- Performance tốt nhất trong các implementation
- Resizable array implementation
- Không đồng bộ hóa nên nhanh hơn Stack class

**Nhược điểm:**

- Không thread-safe
- Không cho phép null elements

## 3. LinkedList

```Java
Deque<Integer> stack = new LinkedList<>();
// Các method tương tự ArrayDeque
```

**Ưu điểm:**

- Cho phép null elements
- Memory usage hiệu quả với dữ liệu lớn do không cần resize array

**Nhược điểm:**

- Performance kém hơn ArrayDeque
- Tốn bộ nhớ hơn do phải lưu reference

# So sánh Performance

1. **Thêm/Xóa phần tử:**
    - ArrayDeque: O(1)
    - LinkedList: O(1)
    - Stack: O(1) nhưng chậm hơn do synchronization
2. **Bộ nhớ:**
    - ArrayDeque: Cần resize array khi đầy
    - LinkedList: Tốn bộ nhớ cho node references
    - Stack: Tương tự ArrayDeque
3. **Use cases phù hợp:**
    - ArrayDeque: Phù hợp nhất cho hầu hết trường hợp
    - LinkedList: Khi cần thao tác nhiều ở giữa collection
    - Stack class: Legacy code hoặc khi cần thread-safe

# Code ví dụ so sánh

```Java
// Stack class
Stack<Integer> stack1 = new Stack<>();
stack1.push(1);
stack1.pop();
stack1.peek();

// ArrayDeque
Deque<Integer> stack2 = new ArrayDeque<>();
stack2.addFirst(1);    // push
stack2.removeFirst();  // pop
stack2.peekFirst();    // peek

// LinkedList
Deque<Integer> stack3 = new LinkedList<>();
stack3.addFirst(1);    // push
stack3.removeFirst();  // pop
stack3.peekFirst();    // peek
```

# Khuyến nghị sử dụng

1. Với ứng dụng thông thường: Sử dụng **ArrayDeque**
2. Cần thread-safe: Sử dụng **concurrent collections** thay vì Stack class
3. Cần thao tác với null elements: Sử dụng **LinkedList**

Bạn có thắc mắc gì về các implementation này không? Tôi sẽ giải thích thêm.