## 1. Khái niệm Map

Map (còn gọi là Dictionary hoặc Associative Array) là cấu trúc dữ liệu lưu trữ các cặp key-value, trong đó mỗi key là duy nhất và được liên kết với một giá trị.

## 2. Đặc điểm chính của Map

- **Key-Value Pairs**: Lưu trữ dữ liệu dưới dạng cặp khóa-giá trị
- **Unique Keys**: Mỗi khóa trong Map là duy nhất
- **Truy cập nhanh**: Cho phép truy xuất dữ liệu nhanh chóng dựa trên khóa
- **Kích thước động**: Có thể mở rộng hoặc thu nhỏ khi thêm/xóa phần tử

## 3. Loại Map chính

- **Ordered Map**: Duy trì thứ tự các phần tử (thường dùng cây nhị phân cân bằng)
- **Unordered Map**: Không duy trì thứ tự (thường dùng bảng băm)

## 4. Các phép toán cơ bản

- **Insert**: Thêm cặp key-value
- **Retrieve**: Lấy giá trị thông qua key
- **Update**: Cập nhật giá trị của key
- **Delete**: Xóa cặp key-value
- **Lookup**: Kiểm tra sự tồn tại của key
- **Iteration**: Duyệt qua các phần tử

## 5. Map trong Java

```Java
// Khởi tạo HashMap
HashMap<String, Integer> map = new HashMap<>();

// Thêm phần tử
map.put("apple", 100);
map.put("banana", 200);

// Lấy giá trị
int value = map.get("apple"); // 100

// Cập nhật giá trị
map.put("apple", 150);

// Kiểm tra tồn tại
boolean exists = map.containsKey("apple"); // true

// Xóa phần tử
map.remove("banana");

// Duyệt map
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

## 6. Các biến thể Map trong Java

- **HashMap**: Triển khai không có thứ tự, truy xuất nhanh O(1)
- **TreeMap**: Duy trì thứ tự các key (sorted), thời gian truy xuất O(log n)
- **LinkedHashMap**: Duy trì thứ tự chèn, thời gian truy xuất O(1)
- **ConcurrentHashMap**: Thread-safe, phù hợp cho ứng dụng đa luồng

## 7. Ưu điểm của Map

- Truy xuất dữ liệu nhanh (O(1) cho HashMap hoặc O(log n) cho TreeMap)
- Linh hoạt trong lưu trữ dữ liệu
- Biểu diễn trực quan các mối quan hệ dữ liệu

## 8. Nhược điểm của Map

- Tốn bộ nhớ hơn so với mảng
- Có thể xảy ra xung đột trong triển khai bảng băm
- Một số triển khai yêu cầu thứ tự của keys

## 9. Ứng dụng của Map

- Lập chỉ mục và truy xuất dữ liệu
- Nhóm và phân loại dữ liệu
- Định tuyến mạng
- Thuật toán đồ thị
- Bộ nhớ cache
- Cơ sở dữ liệu
- Trình biên dịch