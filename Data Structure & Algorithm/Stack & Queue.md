# Stack (Ngăn xếp)
## Khái niệm
- Stack là cấu trúc dữ liệu hoạt động theo nguyên tắc LIFO (Last In First Out)
- Giống như chồng đĩa: đĩa đặt sau cùng sẽ được lấy ra trước tiên
- Các thao tác cơ bản:
    - Push: Thêm phần tử vào đỉnh stack
    - Pop: Lấy và xóa phần tử ở đỉnh stack
    - Peek/Top: Xem phần tử ở đỉnh stack mà không xóa
    - isEmpty: Kiểm tra stack có rỗng không
## Ứng dụng của Stack
- Quản lý lịch sử duyệt web (Back/Forward)
- Quản lý việc Undo/Redo trong editor
- Đánh giá biểu thức toán học
- Quản lý lời gọi hàm trong chương trình
- Parse cú pháp trong compiler
# Queue (Hàng đợi)
## Khái niệm
- Queue là cấu trúc dữ liệu hoạt động theo nguyên tắc FIFO (First In First Out)
- Giống như hàng người đợi mua vé: người đến trước được phục vụ trước
- Các thao tác cơ bản:
    - Enqueue: Thêm phần tử vào cuối queue
    - Dequeue: Lấy và xóa phần tử ở đầu queue
    - Front: Xem phần tử ở đầu queue mà không xóa
    - isEmpty: Kiểm tra queue có rỗng không
## Ứng dụng của Queue
- Quản lý in ấn (Print Queue)
- Xử lý yêu cầu trong web server
- Buffer trong xử lý stream
- BFS (Breadth-First Search) trong đồ thị
- Xử lý tác vụ theo thứ tự thời gian
# So sánh Stack và Queue

| Đặc điểm      | Stack      | Queue           |
| ------------- | ---------- | --------------- |
| Nguyên tắc    | LIFO       | FIFO            |
| Thêm phần tử  | Đỉnh (top) | Cuối (rear)     |
| Lấy phần tử   | Đỉnh (top) | Đầu (front)     |
| Ví dụ thực tế | Chồng đĩa  | Hàng đợi mua vé |