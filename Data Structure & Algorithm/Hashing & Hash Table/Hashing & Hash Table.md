# Giới thiệu về Hashing

Hashing là quá trình chuyển đổi dữ liệu đầu vào thành một chuỗi có độ dài cố định thông qua hàm băm, nhằm xác định vị trí lưu trữ dữ liệu trong cấu trúc dữ liệu.

# Tổng quan về Cấu trúc dữ liệu Hash

- Là cấu trúc dữ liệu phổ biến thứ hai sau mảng
- Hỗ trợ các thao tác tìm kiếm, thêm, xóa với độ phức tạp O(1)
- Ứng dụng: Database Indexing, Mã hóa, Bộ nhớ đệm, Symbol Table
- Hai dạng hash phổ biến:
    - Hash Set: Tập hợp các key duy nhất
    - Hash Map: Tập hợp các cặp key-value, với key duy nhất

# Trường hợp không nên dùng Hash

- Khi cần duy trì dữ liệu đã sắp xếp (dùng Self Balancing BST)
- Khi cần tìm kiếm tiền tố với strings (dùng Trie)
- Khi cần các thao tác floor và ceiling (dùng Self Balancing BST)

# Thành phần của Hashing

1. Key: Dữ liệu đầu vào (string, số,...)
2. Hash Function: Hàm chuyển đổi key thành index
3. Hash Table: Mảng lưu trữ dữ liệu theo index

# Cách Hashing hoạt động

Ví dụ với tập hợp {"ab", "cd", "efg"}:

1. Gán giá trị số cho mỗi ký tự (a=1, b=2,...)
2. Tính tổng giá trị: "ab"=3, "cd"=7, "efg"=18
3. Với bảng kích thước 7, dùng phép mod để tính vị trí:
    - "ab": 3 mod 7 = 3
    - "cd": 7 mod 7 = 0
    - "efg": 18 mod 7 = 4

# Hàm Hash (Hash Function)

- Ánh xạ key thành index trong hash table
- Các loại hàm hash:
    1. Division Method
    2. Mid Square Method
    3. Folding Method
    4. Multiplication Method

# Đặc điểm của hàm hash tốt

- Hiệu quả trong tính toán
- Phân phối đều các key
- Giảm thiểu va chạm
- Duy trì load factor thấp

# Va chạm (Collision)

- Xảy ra khi hai key khác nhau cho cùng giá trị hash
- Ví dụ: "ab" và "ba" có cùng tổng giá trị
- Hai phương pháp xử lý:
    1. Separate Chaining
    2. Open Addressing

Tham khảo thêm dưới đây:

[[Hash Collision|Hash Collision]]

# Load Factor

- Công thức: Số phần tử / Kích thước bảng
- Dùng để đánh giá hiệu quả của hàm băm
- Ngưỡng mặc định: 0.75

# Rehashing

- Thực hiện khi load factor vượt ngưỡng
- Tăng kích thước bảng (thường gấp đôi)
- Băm lại toàn bộ dữ liệu vào bảng mới

# Kết luận

Hashing giải quyết bài toán tìm kiếm nhanh trong tập dữ liệu lớn. Thay vì duyệt tuần tự, hashing giúp thu hẹp phạm vi tìm kiếm, tăng hiệu suất xử lý.

Bạn có thể xem đây như một "bản đồ" để định vị dữ liệu nhanh chóng, thay vì phải tìm kiếm từng ngõ ngách một cách thủ công.