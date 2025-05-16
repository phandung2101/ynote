> Tài liệu tham khảo: [https://www.geeksforgeeks.org/heap-data-structure](https://www.geeksforgeeks.org/heap-data-structure/?ref=outind)

# Cấu trúc dữ liệu Heap trong Java

Heap là cấu trúc dữ liệu dạng cây nhị phân hoàn chỉnh với tính chất:

- **Max Heap**: Mỗi nút cha lớn hơn hoặc bằng các nút con
- **Min Heap**: Mỗi nút cha nhỏ hơn hoặc bằng các nút con

Dưới đây là triển khai cơ bản của Min Heap bằng Java:

```Java
public class MinHeap {
    private int[] heap;
    private int size;
    private int capacity;

    public MinHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.heap = new int[capacity];
    }

    private int parent(int i) { return (i - 1) / 2; }
    private int leftChild(int i) { return 2 * i + 1; }
    private int rightChild(int i) { return 2 * i + 2; }

    // Lấy phần tử nhỏ nhất (đỉnh heap)
    public int getMin() {
        if (size <= 0) throw new IllegalStateException("Heap rỗng");
        return heap[0];
    }

    // Thêm phần tử mới
    public void insert(int value) {
        if (size == capacity) throw new IllegalStateException("Heap đầy");

        // Chèn phần tử mới vào cuối
        heap[size] = value;
        int current = size;
        size++;

        // Heapify lên trên
        while (current > 0 && heap[current] < heap[parent(current)]) {
            swap(current, parent(current));
            current = parent(current);
        }
    }

    // Lấy và xóa phần tử nhỏ nhất
    public int extractMin() {
        if (size <= 0) throw new IllegalStateException("Heap rỗng");

        int min = heap[0];
        heap[0] = heap[size - 1];
        size--;

        // Heapify xuống dưới
        heapifyDown(0);

        return min;
    }

    private void heapifyDown(int i) {
        int smallest = i;
        int left = leftChild(i);
        int right = rightChild(i);

        if (left < size && heap[left] < heap[smallest])
            smallest = left;

        if (right < size && heap[right] < heap[smallest])
            smallest = right;

        if (smallest != i) {
            swap(i, smallest);
            heapifyDown(smallest);
        }
    }

    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
}
```

## Ví dụ sử dụng:

```Java
MinHeap minHeap = new MinHeap(10);
minHeap.insert(5);
minHeap.insert(3);
minHeap.insert(8);
minHeap.insert(1);

System.out.println("Min: " + minHeap.getMin());         // 1
System.out.println("Extract: " + minHeap.extractMin()); // 1
System.out.println("New min: " + minHeap.getMin());     // 3
```

## Độ phức tạp

- Lấy min/max: O(1)
- Chèn/Xóa: O(log n)
- Xây dựng heap: O(n)

Heap thường được ứng dụng trong Heap Sort, thuật toán Dijkstra và Priority Queue.​​​​​​​​​​​​​​​​