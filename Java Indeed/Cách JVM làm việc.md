JVM chạy ứng dụng như run-time engine. JVM là nơi thực hiện việc gọi tới method main trong java code.
Java là WORM (Write one read many) có thể chạy nhiều os tất cả nhờ sử dụng JVM.
![[JVM.png]]
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
- **Resolution**: Quá trình này thay thế 
