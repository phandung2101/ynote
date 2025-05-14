JVM chạy ứng dụng như run-time engine. JVM là nơi thực hiện việc gọi tới method main trong java code.
Java là WORM (Write one read many) có thể chạy nhiều os tất cả nhờ sử dụng JVM.
![[jvm.png]]
# Class Loader subsystem
Chịu trách nhiệm cho 3 việc chính:
- Loading 
- Linking
- Initialization
## Loading
Đọc .class file tạo ra binary data và lưu vào **Method Area**. Mỗi class file thì sẽ lưu các thông tin:
- tên đầy đủ class và parent của nó
- class, interface, enum liên quan
- modifier, variables và thông tin method, ...
Sau khi load .class file xong thì tạo ra 1 loại object đại diện cho Class đấy và lưu vào heap memory. Object là loại Class được định nghĩa trước tại java.lang. Đối tượng Class này được sử dụng để lấy thông tin về class đó như là class name, parent name, method và variable info. Để lấy thông tin này chúng ta có thể sử dụng `getClass()` của Object class.
## Linking 
Thực hiện quá trình verification, preparation và có thể là cả resolution:
- **Verification**: đảm bảo tính đúng đắn của .class file dựa vào các tiêu chí như format đúng chưa, valid complier hay không. Nếu thất bại chúng ta sẽ nhận lỗi run-time exception java.lang.VerifyError. Hoạt động này được thực hiện bởi ByteCodeVerifier. Khi quá trình này hoàn thành thì class file đã sẵn sàng để compilation.
- **Preparation**: JVM allocate memory cho các class static variable và khởi tạo bộ nhớ cho giá trị default
- **Resolution (có thể chạy hoặc không)**: Quá trình này thay thế symbolic references bằng direct references.
Chú thích: symbolic references là tham chiếu đến các method thông qua nơi sử dụng. ví dụ như
```java
String s = "hello";
int len = s.length();
```
Symbolic reference trong bytecode sẽ được tham chiếu đến class `java/lang/String` với method `length`. Tham chiếu này sẽ được tìm trong method area theo class `String` và tìm địa chỉ bộ nhớ thực tế (direct reference) tới method/field. Có thể thay thế trực tiếp **Constant Pool** của class đang chạy bằng direct reference.
## Initialization
Tất cả static variable sẽ được assign giá trị trong class code và static block. Quá trình này thực hiện theo thứ tự top to bottom và theo thứ tự class cha đến class con. Sẽ có 3 class loaders:
1. **Bootstrap class loader**: mỗi JVM implementation đều có một bootstrap class loader. Nó thực hiện load trusted class hay các core java API class trong "JAVA_HOME/lib". Thường được biết tới là bootstrap path. Nó thường được implement bằng các ngôn ngữ native như C/C++.
2. **Extension class loader**: là con của thằng mục 1. có nhiệm vụ load class extension trong "JAVA_HOME/jre/lib/ext" hoặc bất cứ thư mục nào được chỉ định bởi java.ext.dirs. Nó được implement bằng java bởi sun.misc.Launcher$ExtClassLoader class.
3. **System/Application class loader**: con của thằng mục 2. nhiệm vụ load class thuộc application classpath. Sử dụng env var map bởi java.class.path. Implment bằng java bởi sun.misc.Launcher$AppClassLoader class.
*Note: class nếu không tìm thấy ở con sẽ truy ngược về cha. nếu vẫn không tìm thấy sẽ đưa ra lỗi java.lang.ClassNotFoundException.*

# Class Loaders
![[class_loader.png]]
Có 3 loại class loader chính là: (tham khảo thêm ở phần ngay trên)
1. Bootstrap class loader
2. Extension class loader
3. System/Application class loader

Việc load class được thực hiện theo Delegation-Hierarchy principle. Dùng để:
- tránh trùng lặp
- tránh override lại các core library (ví dụ override String chẳng hạn)
# JVM Memory Area
![[jvm_memory_area.png]]
1. **Method Area**: vùng nhớ này được lưu class name, parent class name, method và variables info được lưu bao gồm cả static variables. Chỉ có 1 method area trên mỗi JVM và là shared resource.
2. **Heap Area**: thông tin về tất cả object được lưu tại đây. 1 heap area trên mỗi JVM và shared resource.
3. **Stack Area**: Với mỗi thread, JVM tạo ra 1 run-time stack được lưu tại đây. Không phải shared resource . Sẽ được destroy khi thread đó bị terminated.
4. **PC Registers**: Lưu địa chỉ của **execution instructor** của mỗi thread. Tất nhiên mỗi thread sẽ có 1 PC Register tương ứng.
5. **Native method stack**: Với mỗi thread, có 1 native stack được tạo riêng. Nó lưu **native method location**.
# Execution Engine
Execution Engine thực hiện execute các file .class (bytecode). Đọc line by line, data sử dụng và thông tin đại diện ở nhiều **memory area** khác nhau và thực hiện theo **execution instruction**. Có thể phân ra làm 3 phần:
- **Interpreter**: Nó diễn dịch bytecode line by line và execute. Yếu điểm ở đây là khi method được thực hiện đi thực hiện lại thì phải thực hiện **Interpretation** lại
- **Just-in-time Complier (JIT)**: Được sử dụng để cải thiện hiệu quả của **Interpreter**. Nó compile toàn bộ bytecode and thay đổi nó thành native code để mỗi khi interpreter thấy lặp lại method call thì **JIT** cung cấp direct native code cho phần đấy mà không phải **re-interpretation** bắt buộc nữa -> hiệu quả được cải thiện
- **Garbage Collector**: destroy các un-referenced object
# Java Native Interface (JNI)
Là interface thực hiện tương tác với **Native Method Libraries** và cung cấp các navite library C/C++ cần thiết để thực thi. Nó cho phép JVM call đến C/C++ lib và được call bở C/C++ lib cần thiết cho phần cứng.
# Native Method Libraries
Tập hợp native libraries cần thiết để thực thi native methods. Nó bao gồm các thư viện được viết bằng C và C++.
