## 1. Khái niệm
Set là cấu trúc dữ liệu lưu trữ tập hợp các phần tử duy nhất (không trùng lặp). Mỗi phần tử trong Set chỉ xuất hiện một lần và thường được sắp xếp theo thứ tự nhất định.
## 2. Đặc điểm chính
- **Phần tử duy nhất**: Không cho phép trùng lặp
- **Không theo thứ tự**: Trong một số triển khai, phần tử không được lưu trữ theo thứ tự chèn
- **Bất biến**: Giá trị phần tử không thể thay đổi sau khi được thêm vào Set
- **Kích thước động**: Có thể mở rộng và thu nhỏ khi thêm/xóa phần tử
## 3. Các loại Set
- **Unordered Set**: Triển khai bằng bảng băm, truy xuất nhanh O(1)
- **Ordered Set**: Triển khai bằng cây nhị phân cân bằng, phần tử được sắp xếp, truy xuất O(log n)
## 4. Triển khai trong các ngôn ngữ
- **Java**: HashSet, TreeSet, LinkedHashSet
- **C++**: std::set, std::unordered_set, std::multiset
- **Python**: set, frozenset
- **JavaScript**: Set
- **C#**: HashSet
## 5. Các phép toán cơ bản

```Java
// Khởi tạo Set trong Java
HashSet<Integer> set = new HashSet<>();

// Thêm phần tử
set.add(10);
set.add(20);
set.add(10); // Phần tử 10 đã tồn tại, không được thêm lại

// Kiểm tra tồn tại
boolean exists = set.contains(10); // true

// Xóa phần tử
set.remove(20);

// Kích thước
int size = set.size(); // 1

// Duyệt Set
for (Integer element : set) {
    System.out.println(element);
}

// Xóa tất cả phần tử
set.clear();
```
## 6. Độ phức tạp thời gian

| Phép toán | HashSet (Unordered) | TreeSet (Ordered) |
| --------- | ------------------- | ----------------- |
| Thêm      | O(1) trung bình     | O(log n)          |
| Xóa       | O(1) trung bình     | O(log n)          |
| Tìm kiếm  | O(1) trung bình     | O(log n)          |
## 7. Ưu điểm
- Truy xuất và tìm kiếm nhanh
- Loại bỏ trùng lặp tự động
- Hỗ trợ các phép toán tập hợp (giao, hợp, hiệu)
- Linh hoạt về kích thước
## 8. Nhược điểm
- Tốn bộ nhớ hơn so với mảng
- Không thể truy cập phần tử theo chỉ mục
- Có thể xảy ra xung đột trong triển khai bảng băm
## 9. Ứng dụng
- Loại bỏ các phần tử trùng lặp
- Kiểm tra thành viên trong tập hợp
- Thực hiện các phép toán tập hợp (giao, hợp, hiệu)
- Thuật toán đồ thị
- Bộ nhớ cache
- Lập chỉ mục cơ sở dữ liệu