# Nguồn tài liệu

> [!info] Big O Notation Tutorial - A Guide to Big O Analysis - GeeksforGeeks  
> Big O notation is a mathematical tool used in computer science to express the upper bound of an algorithm's time or space complexity, allowing for the comparison of algorithm efficiency in worst-case scenarios.  
> [https://www.geeksforgeeks.org/analysis-algorithms-big-o-analysis/](https://www.geeksforgeeks.org/analysis-algorithms-big-o-analysis/)  

# Khái niệm

- Mô tả độ phức tạp về **thời gian (time complexity)** hoặc độ phức tạp về **không gian (space complexity)** của các thuật toán.
- Thể hiện **cận trên (upper bound)** của thuật toán hay còn được hiểu giới hạn tối đa dựa trên input

# Tính chất

## Phản xạ (Reflexivity):

- Đơn giản là: một hàm luôn là Big O của chính nó
- Giống như bạn nói "Tôi là chính tôi"
- Ví dụ: n² luôn là O(n²)

## Bắc cầu (Transitivity):

- Nếu A ≤ B và B ≤ C thì A ≤ C
- Trong Big O: nếu f(n) không tăng nhanh hơn g(n), và g(n) không tăng nhanh hơn h(n), thì f(n) chắc chắn không tăng nhanh hơn h(n)
- Ví dụ: n² ≤ n³ ≤ n⁴, nên n² chắc chắn ≤ n⁴

## Hằng số (Constant Factor):

- Nhân một hàm với hằng số không làm thay đổi Big O
- 2n, 3n, 100n đều là O(n)
- Vì khi n đủ lớn, hằng số không còn quan trọng

## Tổng (Sum Rule):

- Big O của tổng không lớn hơn Big O của thành phần lớn nhất
- Ví dụ: O(n) + O(n²) = O(n²)
- Giống như trong toán: 5 + 100 không lớn hơn 100

## Nhân (Product Rule):

- Big O của tích bằng tích của các Big O
- O(n) * O(n²) = O(n³)
- Dễ hiểu vì khi nhân, độ phức tạp sẽ cộng lên

## Hợp thành (Composition):

- Khi một hàm là đầu vào của hàm khác
- f(g(n)) nghĩa là g(n) chạy trước, kết quả đưa vào f(n)
- Độ phức tạp sẽ lồng nhau

# Mức độ phức tạp thường gặp

![[/image.png|image.png]]

  

## O(1) - Constant: Thời gian thực thi không đổi

Dễ quá skip

## O(n) - Linear: Tăng tuyến tính với n

```C++
bool findElement(int arr[], int n, int key)
{
    for (int i = 0; i < n; i++) {
        if (arr[i] == key) {
            return true;
        }
    }
    return false;
}
```

## O(log n) - Logarithmic: Tăng chậm khi n tăng

```C++
int binarySearch(int arr[], int l, int r, int x)
{
    if (r >= l) {
        int mid = l + (r - l) / 2;
        if (arr[mid] == x)
            return mid;
        if (arr[mid] > x)
            return binarySearch(arr, l, mid - 1, x);
        return binarySearch(arr, mid + 1, r, x);
    }
    return -1;
}
```

## O(n log n) - Linearithmic: Hơi nhanh hơn tuyến tính

TLTR: Hiểu đơn giản độ phức tạp (n log n) thường xuất hiện khi có 2 thuật toán lồng nhau với thuật toán trên có độ phức tạp n và thuật toán trong là log n. Hoặc là ngược lại. Ví dụ đơn giản nhất: 2 vòng for. Vòng for ngoài duyệt n lần. Vòng for trong duyệt log n lần.

```Java
public class MergeSort {
    public static void mergeSort(int[] arr) {
        // Nếu mảng có 1 phần tử hoặc rỗng thì đã sắp xếp
        if (arr.length <= 1) {
            return;
        }

        // Chia mảng làm 2 phần
        int mid = arr.length / 2;
        int[] left = new int[mid];
        int[] right = new int[arr.length - mid];

        // Copy dữ liệu vào 2 mảng con
        for (int i = 0; i < mid; i++) {
            left[i] = arr[i];
        }
        for (int i = mid; i < arr.length; i++) {
            right[i - mid] = arr[i];
        }

        // Đệ quy sắp xếp 2 mảng con
        mergeSort(left);
        mergeSort(right);

        // Trộn 2 mảng đã sắp xếp
        merge(arr, left, right);
    }

    private static void merge(int[] arr, int[] left, int[] right) {
        int i = 0, j = 0, k = 0;
        
        // So sánh và trộn 2 mảng con
        while (i < left.length && j < right.length) {
            if (left[i] <= right[j]) {
                arr[k++] = left[i++];
            } else {
                arr[k++] = right[j++];
            }
        }

        // Copy các phần tử còn lại của mảng trái (nếu có)
        while (i < left.length) {
            arr[k++] = left[i++];
        }

        // Copy các phần tử còn lại của mảng phải (nếu có)
        while (j < right.length) {
            arr[k++] = right[j++];
        }
    }

    // Phương thức main để test
    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        System.out.println("Mảng ban đầu:");
        printArray(arr);

        mergeSort(arr);

        System.out.println("\nMảng sau khi sắp xếp:");
        printArray(arr);
    }

    // Phương thức in mảng
    private static void printArray(int[] arr) {
        for (int i : arr) {
            System.out.print(i + " ");
        }
        System.out.println();
    }
}
```

## O(n²) - Quadratic: Tăng theo bình phương

```C++
void bubbleSort(int arr[], int n)
{
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(&arr[j], &arr[j + 1]);
            }
        }
    }
}
```

## O(nᵏ) - Polynomial

Hiểu thuật toán này có nhận vào hằng số k và phải duyệt lặp đúng bằng k lần n. Phổ biến nhất là O(n³) dùng trong các thuật toán xử lý cặp ma trận 2d hoặc xử lý ma trận 3d

```C++
void multiply(int mat1[][N], int mat2[][N], int res[][N])
{
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            res[i][j] = 0;
            for (int k = 0; k < N; k++)
                res[i][j] += mat1[i][k] * mat2[k][j];
        }
    }
}
```

## O(2ⁿ) - Exponential: Tăng rất nhanh

```C++
// Fibonacci naive way
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n-1) + fibonacci(n-2);
}
```

## O(n!) - Factorial: Max ping

```C++
void permute(int* a, int l, int r)
{
    if (l == r) {
        for (int i = 0; i <= r; i++) {
            cout << a[i] << " ";
        }
        cout << endl;
    }
    else {
        for (int i = l; i <= r; i++) {
            swap(a[l], a[i]);
            permute(a, l + 1, r);
            swap(a[l], a[i]); // backtrack
        }
    }
}
```

# So sánh độ phức tạp trên thực tế

|**n**|**log(n)**|**n**|**n * log(n)**|**n^2**|**2^n**|**n!**|
|---|---|---|---|---|---|---|
|10|1|10|10|100|1024|3628800|
|20|2.996|20|59.9|400|1048576|2.432902e+1818|

# Các thuật toán nổi bật

|Độ phức tạp|Ký hiệu|Thuật toán nổi bật|
|---|---|---|
|Logarithmic|O(log n)|Binary Search|
|Linear|O(n)|Linear Search|
|Superlinear|O(n log n)|Heap Sort, Merge Sort|
|Polynomial|O(n^c)|Strassen’s Matrix Multiplication, Bubble Sort, Selection Sort, Insertion Sort, Bucket Sort|
|Exponential|O(c^n)|Tower of Hanoi|
|Factorial|O(n!)|Determinant Expansion by Minors, Brute force Search algorithm for Traveling Salesman Problem|

# Thời gian thực tế

|**Big O Notation Classes**|**f(n)**|**Big O Analysis (number of operations) for n = 10**|**Execution Time (1 instruction/μsec)**|
|---|---|---|---|
|**constant**|$O(1)$|1|1 μsec|
|**logarithmic**|$O(log(n))$|3.32|3 μsec|
|**linear**|$O(n)$|10|10 μsec|
|**O(nlogn)**|$O(n log(n))$|33.2|33 μsec|
|**quadratic**|$O(n^2)$|102|100 μsec|
|**cubic**|$O(n^3)$|103|1msec|
|**exponential**|$O(2^n)$|1024|10 msec|
|**factorial**|$O(n!)$|10!|3.6288 sec|