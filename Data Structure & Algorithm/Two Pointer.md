  

## Tổng quan

Kỹ thuật Two Pointers là phương pháp sử dụng hai con trỏ để duyệt mảng, thường được dùng để giải quyết các bài toán tối ưu, giúp giảm độ phức tạp từ O(n²) xuống O(n).

## Các dạng Two Pointers phổ biến

### 1. Two Pointers từ hai đầu mảng

```Java
public static boolean findPairSum(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;

    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) {
            return true;
        }
        if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return false;
}
```

### 2. Fast và Slow Pointers

```Java
public static ListNode findMiddle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

## Ứng dụng phổ biến

- Tìm cặp số có tổng bằng X trong mảng đã sắp xếp
- Đảo ngược mảng
- Tìm điểm giữa của linked list
- Kiểm tra palindrome
- Loại bỏ phần tử trùng lặp

## Ví dụ thực tế

Cho mảng đã sắp xếp: [2, 7, 11, 15] và target = 9

1. Khởi tạo: left = 0 (số 2), right = 3 (số 15)
2. Tính tổng: 2 + 15 = 17 > 9 → right--
3. Tính tổng: 2 + 11 = 13 > 9 → right--
4. Tính tổng: 2 + 7 = 9 = target → Tìm thấy cặp số

Kỹ thuật này giúp giải quyết bài toán với độ phức tạp O(n) thay vì O(n²) như cách duyệt thông thường.