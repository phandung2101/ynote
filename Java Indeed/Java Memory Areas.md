## 1. **Method Area (Class Area)**

- **Chia sẻ cho toàn JVM**, tồn tại suốt vòng đời JVM.
- Lưu: Thông tin lớp (tên, kế thừa, interface...), bytecode các phương thức, biến `static`, constant pool.
- Từ Java 8: Được chuyển sang vùng **Metaspace** (ngoài Heap).

🧪 **Ví dụ:**
```java
class Config { static String appName = "MyApp"; }
System.out.println(Config.appName); // Không cần tạo object
```
## 2. **Heap Area**

- Vùng nhớ lớn nhất, **chia sẻ giữa các thread**.
- Lưu: Các **object** và mảng được tạo bằng `new`.
- Quản lý bởi **Garbage Collector**.

🧪 **Ví dụ:**

```java
Person p1 = new Person(); // new → Heap
p1.name = "Alice";
```
## 3. **Stack Area (Java Stack)**

- **Mỗi thread có 1 stack riêng**, chứa các **stack frame** cho lời gọi hàm.
- Lưu: Biến cục bộ, tham số hàm, địa chỉ trả về, operand stack.
- LIFO: Vào sau – ra trước.
- Lỗi thường gặp: `StackOverflowError`.

🧪 **Ví dụ:**

```java
main() → gọi methodA() → gọi methodB()
// mỗi lời gọi tạo 1 frame mới trên stack
```
## 4. **PC Register (Program Counter)**
- **Mỗi thread có 1 PC riêng**.
- Lưu **địa chỉ bytecode đang/chuẩn bị thực thi**.
- Cho phép thread thực thi độc lập, quản lý context chuyển đổi chính xác.

🧪 **Ví dụ:**

```java
Thread A và B in ra vòng lặp → PC của mỗi thread giữ vị trí riêng biệt trong tiến trình của nó
```
## 5. **Native Method Stack**
- **Mỗi thread có 1 stack riêng**, dành cho **phương thức native (C/C++)** gọi qua JNI.
- Lưu biến local, tham số, dữ liệu tạm khi gọi hàm native.
- Khác biệt với Java Stack.

🧪 **Ví dụ:**  
Gọi `System.loadLibrary()` hay JDBC → JVM dùng Native Stack để xử lý phần C/C++ phía dưới.
## 🎯 Tổng kết

|Vùng nhớ|Gắn với|Chia sẻ?|Lưu trữ|
|---|---|---|---|
|Method Area|JVM|✅|Thông tin class, biến static, bytecode|
|Heap|JVM|✅|Object, mảng (dữ liệu động)|
|Java Stack|Thread|❌|Biến cục bộ, lời gọi phương thức|
|PC Register|Thread|❌|Địa chỉ lệnh bytecode tiếp theo|
|Native Method Stack|Thread|❌|Stack cho phương thức native (JNI)|
