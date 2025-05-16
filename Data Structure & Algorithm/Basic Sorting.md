# Bubble Sort (Sắp xếp nổi bọt)

Bubble sort là thuật toán đơn giản nhất, hoạt động bằng cách so sánh và hoán đổi các cặp phần tử liền kề nếu chúng không đúng thứ tự.

```Java
void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

Ưu điểm:

- Dễ hiểu và cài đặt
- Ít bộ nhớ phụ (in-place sorting)

Nhược điểm:

- Độ phức tạp thời gian: O(n²)
- Không hiệu quả với dữ liệu lớn

# Selection Sort (Sắp xếp chọn)

Selection sort tìm phần tử nhỏ nhất (hoặc lớn nhất) và đặt vào vị trí đầu tiên, sau đó tìm phần tử nhỏ nhất trong các phần tử còn lại.

```Java
void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        int temp = arr[minIdx];
        arr[minIdx] = arr[i];
        arr[i] = temp;
    }
}
```

Ưu điểm:

- Số lần hoán đổi ít hơn bubble sort
- In-place sorting

Nhược điểm:

- Độ phức tạp thời gian: O(n²)
- Không ổn định (không giữ thứ tự ban đầu của các phần tử bằng nhau)

# Insertion Sort (Sắp xếp chèn)

Insertion sort xây dựng mảng đã sắp xếp từng phần tử một, bằng cách chèn từng phần tử vào đúng vị trí trong phần đã sắp xếp.

```Java
void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
```

Ưu điểm:

- Hiệu quả với dữ liệu gần như đã sắp xếp
- Ổn định (stable sorting)
- Thích hợp với dữ liệu nhỏ

Nhược điểm:

- Độ phức tạp thời gian trung bình: O(n²)

# Quick Sort (Sắp xếp nhanh)

Quick sort sử dụng chiến lược chia để trị (divide and conquer), chọn một phần tử làm chốt (pivot) và phân vùng mảng thành hai phần dựa trên pivot.

```Java
void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = (low - 1);
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return i + 1;
}
```

Ưu điểm:

- Độ phức tạp thời gian trung bình: O(n log n)
- Hiệu quả với dữ liệu lớn
- In-place sorting (với cài đặt tối ưu)

Nhược điểm:

- Độ phức tạp thời gian xấu nhất: O(n²) nếu chọn pivot không tốt
- Không ổn định
- Hiệu suất phụ thuộc vào việc chọn pivot

So sánh độ phức tạp:

|   |   |   |   |
|---|---|---|---|
|Thuật toán|Trường hợp tốt nhất|Trung bình|Xấu nhất|
|Bubble Sort|O(n)|O(n²)|O(n²)|
|Selection Sort|O(n²)|O(n²)|O(n²)|
|Insertion Sort|O(n)|O(n²)|O(n²)|
|Quick Sort|O(n log n)|O(n log n)|O(n²)|

Bạn có muốn tìm hiểu thêm về cách hoạt động cụ thể của thuật toán nào không?​​​​​​​​​​​​​​​​