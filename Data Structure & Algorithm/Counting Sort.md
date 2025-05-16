Counting Sort là một thuật toán sắp xếp không dựa trên so sánh (non-comparison), có độ phức tạp thời gian tuyến tính O(n+k), với n là số phần tử và k là khoảng giá trị của dữ liệu.

## Nguyên lý hoạt động

Thuật toán hoạt động dựa trên việc đếm số lần xuất hiện của mỗi phần tử trong mảng đầu vào, sau đó tính toán vị trí chính xác của từng phần tử trong mảng kết quả. Các bước chính:

1. Tìm giá trị lớn nhất trong mảng, gọi là max
2. Tạo một mảng đếm có kích thước max+1, khởi tạo với giá trị 0
3. Đếm số lần xuất hiện của mỗi phần tử và lưu vào mảng đếm
4. Tính tổng tích lũy trong mảng đếm
5. Tạo mảng kết quả và đặt mỗi phần tử vào đúng vị trí dựa trên mảng đếm

## Triển khai

```Java
public static void countingSort(int[] arr) {
    // Tìm giá trị lớn nhất trong mảng
    int max = Arrays.stream(arr).max().getAsInt();

    // Tạo mảng đếm
    int[] count = new int[max + 1];

    // Đếm số lần xuất hiện
    for (int num : arr) {
        count[num]++;
    }

    // Tính tổng tích lũy
    for (int i = 1; i <= max; i++) {
        count[i] += count[i - 1];
    }

    // Tạo mảng kết quả
    int[] output = new int[arr.length];

    // Xây dựng mảng kết quả
    for (int i = arr.length - 1; i >= 0; i--) {
        output[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }

    // Sao chép kết quả vào mảng gốc
    System.arraycopy(output, 0, arr, 0, arr.length);
}
```

## Ưu điểm và nhược điểm

**Ưu điểm:**

- Hiệu quả cao với dữ liệu có phạm vi giá trị nhỏ
- Độ phức tạp thời gian O(n+k), nhanh hơn các thuật toán sắp xếp dựa trên so sánh như Quick Sort và Merge Sort trong những trường hợp nhất định
- Thuật toán sắp xếp ổn định (stable sort)

**Nhược điểm:**

- Chỉ hiệu quả khi khoảng giá trị k không quá lớn so với số lượng phần tử n
- Chỉ áp dụng được cho dữ liệu số nguyên không âm (có thể mở rộng cho số âm và số thực nhưng cần điều chỉnh thêm)
- Cần thêm bộ nhớ cho mảng đếm và mảng kết quả

Counting Sort là lựa chọn tuyệt vời khi sắp xếp các mảng số nguyên có phạm vi giá trị hạn chế.​​​​​​​​​​​​​​​​