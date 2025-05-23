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
## 1. Separate Chaining (Chuỗi phân tách)
**Nguyên lý:**
- Mỗi vị trí trong hash table là một linked list
- Các phần tử có cùng hash value được nối thành chuỗi tại vị trí đó
```Python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None

class HashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size

    def insert(self, key, value):
        index = self.hash_function(key)
        if self.table[index] is None:
            self.table[index] = Node(key, value)
        else:
# Thêm vào đầu linked list
            new_node = Node(key, value)
            new_node.next = self.table[index]
            self.table[index] = new_node
```

**Ưu điểm:**

- Đơn giản để implement
- Không giới hạn số lượng phần tử
- Xóa dễ dàng

**Nhược điểm:**

- Tốn thêm bộ nhớ cho con trỏ
- Có thể tạo chuỗi dài nếu nhiều collision

## 2. Open Addressing (Địa chỉ mở)

**Nguyên lý:**

- Khi có collision, tìm vị trí trống tiếp theo trong bảng
- Tất cả phần tử được lưu trực tiếp trong bảng

### Các phương pháp tìm vị trí:

#### a) Linear Probing (Dò tuyến tính)

```Python
def insert(self, key, value):
    index = self.hash_function(key)
    while self.table[index] is not None:
        index = (index + 1) % self.size# Tìm vị trí trống kế tiếp
    self.table[index] = (key, value)
```

**Vấn đề:** Primary clustering - tạo cụm phần tử liên tiếp

#### b) Quadratic Probing (Dò bậc hai)

```Python
def insert(self, key, value):
    index = self.hash_function(key)
    i = 0
    while self.table[index] is not None:
        i += 1
        index = (index + i*i) % self.size# Nhảy theo bậc 2
    self.table[index] = (key, value)
```

**Cải thiện:** Giảm clustering nhưng không hoàn toàn

#### c) Double Hashing (Băm kép)

```Python
def insert(self, key, value):
    index = self.hash1(key)
    i = 0
    while self.table[index] is not None:
        i += 1
        index = (hash1(key) + i * hash2(key)) % self.size
    self.table[index] = (key, value)
```

**Ưu điểm:** Phân tán tốt nhất trong các phương pháp Open Addressing

## So sánh hai phương pháp:

### Separate Chaining

- Phù hợp khi load factor cao
- Hiệu suất ổn định
- Dễ implement và maintain

### Open Addressing

- Tiết kiệm bộ nhớ hơn
- Tốt khi load factor thấp (<0.7)
- Cache friendly hơn

# Ví dụ thực tế:

```Python
# Implementation của cả hai phương pháp
def separate_chaining_insert(hash_table, key, value):
    index = hash_function(key)
    new_node = Node(key, value)

    if hash_table[index] is None:
        hash_table[index] = new_node
    else:
        current = hash_table[index]
        while current.next:
            if current.key == key:# Update if key exists
                current.value = value
                return
            current = current.next
        current.next = new_node

def open_addressing_insert(hash_table, key, value):
    index = hash_function(key)
    i = 0

    while True:
        probe_index = (index + i*i) % len(hash_table)# Quadratic probing
        if hash_table[probe_index] is None:
            hash_table[probe_index] = (key, value)
            return
        if hash_table[probe_index][0] == key:# Update if key exists
            hash_table[probe_index] = (key, value)
            return
        i += 1
```

# Lời khuyên:

- Sử dụng Separate Chaining khi không biết trước số lượng phần tử
- Sử dụng Open Addressing khi cần tối ưu bộ nhớ và biết trước kích thước dữ liệu

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