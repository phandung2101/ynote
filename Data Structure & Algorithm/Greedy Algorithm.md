# Basic understand
- is an algorithm paradigm that build up solution **piece by piece**
- always choosing next piece that offers the **most obvious** and **immediate benefit**
- used for optimization problems
# Problems identify
- at every step, we can make a choice that **look best at that moment**, and we get the optimal solution to the complete problem
- some popular Greedy Algorithms: Fractional Knapsack, Dijkstra's algorithm, Kruskal's algorithm, Huffman coding and Prim's alogrithm
- the **greedy algorithms** are sometimes also used to get an **approximation for Hard optimization problems**
## Đặc điểm chính

- **Nguyên lý cơ bản**: Luôn chọn lựa tối ưu cục bộ tại mỗi bước
- **Ưu điểm**: Đơn giản, hiệu quả về mặt thời gian (thường có độ phức tạp O(n log n) hoặc thấp hơn)
- **Nhược điểm**: Không luôn đảm bảo tìm ra giải pháp tối ưu toàn cục

## Các bước thực hiện

1. Phân tích vấn đề và xác định cấu trúc tham lam
2. Chứng minh tính đúng đắn của phương pháp tham lam cho vấn đề đang xét
3. Thiết kế thuật toán dựa trên chiến lược tham lam
4. Triển khai và tối ưu hóa

## Ứng dụng phổ biến

- **Bài toán đổi tiền**: Luôn chọn mệnh giá lớn nhất có thể
- **Thuật toán Dijkstra**: Tìm đường đi ngắn nhất trong đồ thị
- **Thuật toán Kruskal và Prim**: Tìm cây khung nhỏ nhất
- **Thuật toán Huffman**: Nén dữ liệu
- **Bài toán lập lịch công việc**: Sắp xếp công việc để tối ưu thời gian hoàn thành

## Ví dụ minh họa

Trong bài toán đổi tiền với các mệnh giá 1, 5, 10, 25, 50, 100, nếu cần đổi 186 đồng:

- Chọn 1 tờ 100 đồng (còn 86)
- Chọn 1 tờ 50 đồng (còn 36)
- Chọn 1 tờ 25 đồng (còn 11)
- Chọn 1 tờ 10 đồng (còn 1)
- Chọn 1 đồng xu 1 đồng (còn 0)

Kết quả: 5 tờ/đồng xu (tối ưu)

## Lưu ý quan trọng

Giải thuật tham lam không phải lúc nào cũng cho kết quả tối ưu. Ví dụ trong bài toán đổi tiền với mệnh giá 1, 3, 4 để đổi 6 đồng:

- Phương pháp tham lam: Chọn 4 + 1 + 1 = 6 đồng (3 đồng xu)
- Phương pháp tối ưu: Chọn 3 + 3 = 6 đồng (2 đồng xu)

Vì vậy, trước khi áp dụng giải thuật tham lam, cần chứng minh tính đúng đắn của phương pháp này cho bài toán cụ thể.

# Ví dụ minh hoạ

## Ví dụ 1: Bài toán lập lịch hoạt động

### Đề bài:

Có n hoạt động, mỗi hoạt động có thời gian bắt đầu s[i] và thời gian kết thúc f[i]. Hãy chọn số lượng hoạt động nhiều nhất mà không bị chồng chéo thời gian.

### Giải pháp tham lam:

1. Sắp xếp các hoạt động theo thời gian kết thúc f[i] tăng dần
2. Chọn hoạt động đầu tiên (có thời gian kết thúc sớm nhất)
3. Loại bỏ tất cả hoạt động chồng chéo với hoạt động vừa chọn
4. Lặp lại bước 2 và 3 cho đến khi hết hoạt động

### Ví dụ cụ thể:

Cho các hoạt động: [(1,4), (3,5), (0,6), (5,7), (3,9), (5,9), (6,10), (8,11), (8,12), (2,14), (12,16)]  
(thời gian bắt đầu, thời gian kết thúc)  

**Bước 1:** Sắp xếp theo thời gian kết thúc: [(1,4), (3,5), (0,6), (5,7), (3,9), (5,9), (6,10), (8,11), (8,12), (2,14), (12,16)]

**Bước 2:** Chọn hoạt động đầu tiên: (1,4)

**Bước 3:** Loại bỏ các hoạt động chồng chéo với (1,4): Loại bỏ (3,5), (0,6), (2,14)

**Bước 4:** Chọn hoạt động tiếp theo trong danh sách còn lại: (5,7)

**Bước 5:** Loại bỏ các hoạt động chồng chéo với (5,7): Loại bỏ (3,9), (5,9), (6,10)

**Bước 6:** Chọn hoạt động tiếp theo: (8,11)

**Bước 7:** Loại bỏ các hoạt động chồng chéo: Loại bỏ (8,12)

**Bước 8:** Chọn hoạt động tiếp theo: (12,16)

**Kết quả:** 4 hoạt động được chọn: (1,4), (5,7), (8,11), (12,16)

### Tại sao dùng Greedy cho bài này:

- Chọn hoạt động kết thúc sớm nhất ở mỗi bước sẽ tạo ra nhiều thời gian trống nhất cho các hoạt động tiếp theo
- Có thể chứng minh toán học rằng phương pháp này luôn cho kết quả tối ưu
- Độ phức tạp thời gian O(n log n) chủ yếu do bước sắp xếp

## Ví dụ 2: Thuật toán Huffman (Nén dữ liệu)

### Đề bài:

Tạo mã hóa nhị phân tối ưu cho tập ký tự, dựa trên tần suất xuất hiện của mỗi ký tự.

### Giải pháp tham lam:

1. Tạo một hàng đợi ưu tiên (priority queue) chứa tất cả ký tự và tần suất của chúng
2. Lặp lại cho đến khi hàng đợi chỉ còn 1 phần tử:
    - Lấy ra hai phần tử có tần suất thấp nhất
    - Tạo một nút mới với tần suất là tổng hai phần tử đó
    - Thêm nút mới vào hàng đợi
3. Xây dựng mã Huffman dựa trên cây tạo ra

### Ví dụ cụ thể:

Cho các ký tự và tần suất: A:5, B:9, C:12, D:13, E:16, F:45

**Bước 1:** Tạo hàng đợi ưu tiên: [A:5, B:9, C:12, D:13, E:16, F:45]

**Bước 2:** Lấy hai phần tử nhỏ nhất (A:5, B:9), tạo nút mới AB:14, hàng đợi: [C:12, D:13, AB:14, E:16, F:45]

**Bước 3:** Lấy hai phần tử nhỏ nhất (C:12, D:13), tạo nút mới CD:25, hàng đợi: [AB:14, E:16, CD:25, F:45]

**Bước 4:** Lấy hai phần tử nhỏ nhất (AB:14, E:16), tạo nút mới ABE:30, hàng đợi: [CD:25, ABE:30, F:45]

**Bước 5:** Lấy hai phần tử nhỏ nhất (CD:25, ABE:30), tạo nút mới CDEAB:55, hàng đợi: [F:45, CDEAB:55]

**Bước 6:** Lấy hai phần tử còn lại (F:45, CDEAB:55), tạo nút gốc với tần suất 100

**Bước 7:** Gán mã nhị phân (0/1) cho mỗi cạnh, từ gốc đến lá:

- A: 1010
- B: 100
- C: 00
- D: 01
- E: 11
- F: 0

### Tại sao dùng Greedy cho bài này:

- Kết hợp các ký tự có tần suất thấp nhất ở mỗi bước tạo ra một cây tối ưu
- Giảm chiều dài trung bình của mã nhị phân, tối ưu hóa kích thước file sau khi nén
- Độ phức tạp O(n log n) hiệu quả cho ứng dụng thực tế

## Ví dụ 3: Bài toán đổi tiền tối ưu

### Đề bài:

Cho một số tiền X và các mệnh giá tiền [v₁, v₂, ..., vₙ]. Tìm số lượng tờ tiền ít nhất để đổi được số tiền X.

### Giải pháp tham lam:

1. Sắp xếp các mệnh giá giảm dần
2. Ở mỗi bước, chọn mệnh giá lớn nhất có thể (không vượt quá số tiền còn lại)

### Ví dụ cụ thể:

Cho các mệnh giá: [1, 5, 10, 25, 100] và số tiền cần đổi: 289

**Bước 1:** Sắp xếp mệnh giá: [100, 25, 10, 5, 1]

**Bước 2:** Chọn càng nhiều tờ 100 càng tốt: 2 tờ 100 (còn lại 89)

**Bước 3:** Chọn càng nhiều tờ 25 càng tốt: 3 tờ 25 (còn lại 14)

**Bước 4:** Chọn càng nhiều tờ 10 càng tốt: 1 tờ 10 (còn lại 4)

**Bước 5:** Chọn càng nhiều tờ 5 càng tốt: 0 tờ 5 (còn lại 4)

**Bước 6:** Chọn càng nhiều tờ 1 càng tốt: 4 tờ 1 (còn lại 0)

**Kết quả:** Tổng cộng 10 tờ tiền (2 tờ 100, 3 tờ 25, 1 tờ 10, 4 tờ 1)

### Tại sao dùng Greedy cho bài này:

- Với hệ thống tiền tệ chuẩn (như USD), chiến lược tham lam luôn cho kết quả tối ưu
- Đơn giản và hiệu quả với độ phức tạp O(n + log(X)) (với n là số lượng mệnh giá)
- **Chú ý:** Không phải hệ thống mệnh giá nào cũng áp dụng được tham lam. Ví dụ với mệnh giá [1, 3, 4] và số tiền 6, giải thuật tham lam sẽ cho kết quả 3 đồng (4+1+1) trong khi kết quả tối ưu là 2 đồng (3+3).

Giải thuật tham lam được ưa chuộng trong nhiều bài toán vì tính đơn giản, hiệu quả về mặt thời gian thực thi, và thường cho kết quả chấp nhận được hoặc tối ưu khi điều kiện bài toán phù hợp.