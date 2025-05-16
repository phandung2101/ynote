## 1. Linear Search (Tìm kiếm tuyến tính)

Là thuật toán tìm kiếm cơ bản nhất, hoạt động bằng cách duyệt tuần tự từng phần tử trong mảng cho đến khi tìm thấy kết quả mong muốn.

### Đặc điểm chính:

- Độ phức tạp thời gian: O(n)
- Không yêu cầu mảng phải sắp xếp
- Phù hợp với mảng có kích thước nhỏ

### Code minh họa:

```Java
public static int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;  // Trả về vị trí nếu tìm thấy
        }
    }
    return -1;  // Trả về -1 nếu không tìm thấy
}
```

## 2. Binary Search (Tìm kiếm nhị phân)

Là thuật toán tìm kiếm nâng cao, hoạt động dựa trên nguyên tắc chia để trị bằng cách liên tục chia đôi khoảng tìm kiếm.

### Đặc điểm chính:

- Độ phức tạp thời gian: O(log n)
- Yêu cầu mảng phải được sắp xếp trước
- Hiệu quả với mảng có kích thước lớn

### Code minh họa:

```Java
public static int binarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            return mid;
        }
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

## So sánh hai thuật toán

### Linear Search

- **Ưu điểm**: Đơn giản, dễ implement, không cần sắp xếp dữ liệu
- **Nhược điểm**: Không hiệu quả với dữ liệu lớn, phải duyệt hết mảng trong trường hợp xấu nhất

### Binary Search

- **Ưu điểm**: Hiệu quả với dữ liệu lớn, thời gian tìm kiếm nhanh
- **Nhược điểm**: Yêu cầu dữ liệu phải được sắp xếp trước, tốn thêm chi phí sắp xếp

## Ví dụ thực tế

Cho mảng đã sắp xếp: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]

Tìm giá trị 23:

- **Linear Search**: Phải duyệt 6 phần tử (2 → 5 → 8 → 12 → 16 → 23)
- **Binary Search**: Chỉ cần 3 bước
    1. So sánh với 16 (giữa mảng) → tìm bên phải
    2. So sánh với 56 → tìm bên trái
    3. Tìm thấy 23