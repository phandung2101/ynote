> **Boyer-Moore Voting Algorithm**

## Định nghĩa phần tử đa số

Phần tử đa số là phần tử xuất hiện nhiều hơn n/2 lần trong mảng n phần tử.

Ví dụ: Trong mảng [2,2,1,1,1,2,2], số 2 xuất hiện 4 lần trong mảng 7 phần tử, nên 2 là phần tử đa số.

## Ý tưởng thuật toán

Nôm na sẽ có nhiều nhóm mỗi nhóm là 1 đội quân. Mỗi khi mỗi quân A gặp quân B thì cả 2 đều chết. Thì sao khi giao chiến thì chỉ tồn tại đội quân đông nhất ⇒ là quân cần tìm.

Thuật toán sử dụng 2 biến:

- candidate: lưu ứng viên hiện tại
- count: đếm số lần xuất hiện

```Python
def findMajority(arr):
    candidate = arr[0]
    count = 1

    for num in arr[1:]:
        if count == 0:
            candidate = num

        if num == candidate:
            count += 1
        else:
            count -= 1

    return candidate
```

## Ví dụ minh họa

Hãy xem xét mảng: [2,2,1,1,1,2,2]

Các bước thực hiện:

1. candidate = 2, count = 1
2. Gặp 2: count = 2
3. Gặp 1: count = 1
4. Gặp 1: count = 0
5. Gặp 1: candidate = 1, count = 1
6. Gặp 2: count = 0
7. Gặp 2: candidate = 2, count = 1

Kết quả: 2 là phần tử đa số

## Tại sao thuật toán hoạt động?

- Nếu có phần tử đa số, nó xuất hiện > n/2 lần
- Khi ta "loại bỏ" các cặp phần tử khác nhau (count giảm)
- Phần tử đa số luôn còn lại cuối cùng vì nó xuất hiện nhiều nhất

## Độ phức tạp

- Thời gian: O(n)
- Không gian: O(1)

## Bài tập thực hành

Hãy thử giải bài tập sau:

```Plain
Input: [3,2,3]
Output: 3

Input: [2,2,1,1,1,2,2]
Output: 2
```

## Lưu ý quan trọng

1. Thuật toán giả định rằng phần tử đa số luôn tồn tại
2. Trong thực tế, nên kiểm tra lại kết quả bằng cách đếm số lần xuất hiện
3. Không áp dụng được cho trường hợp tìm phần tử xuất hiện nhiều nhất (không phải đa số)

Các bạn có câu hỏi gì không?