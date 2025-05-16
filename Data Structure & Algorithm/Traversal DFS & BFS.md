# Depth-First Search (DFS)

DFS là thuật toán duyệt đồ thị bằng cách đi sâu vào một nhánh càng xa càng tốt trước khi quay lại và khám phá các nhánh khác.

### Nguyên lý hoạt động:

1. Bắt đầu từ một đỉnh gốc và đánh dấu nó đã được thăm
2. Khám phá một đỉnh kề chưa được thăm
3. Tiếp tục khám phá sâu từ đỉnh đó
4. Khi không còn đỉnh chưa thăm kề với đỉnh hiện tại, thuật toán quay lui
5. Quá trình tiếp tục cho đến khi tất cả các đỉnh đều được thăm

### Cài đặt:

- Sử dụng đệ quy (stack ngầm định)
- Hoặc sử dụng một stack rõ ràng

### Ứng dụng:

- Tìm đường đi trong mê cung
- Giải các bài toán tìm kiếm có giới hạn
- Sắp xếp topo trong đồ thị có hướng
- Tìm thành phần liên thông mạnh

# Breadth-First Search (BFS)

BFS là thuật toán duyệt đồ thị bằng cách khám phá tất cả các đỉnh ở cùng một mức (khoảng cách từ gốc) trước khi di chuyển đến mức tiếp theo.

### Nguyên lý hoạt động:

1. Bắt đầu từ một đỉnh gốc và đánh dấu nó đã được thăm
2. Thăm tất cả các đỉnh kề với đỉnh gốc
3. Sau đó mới di chuyển đến các đỉnh cách gốc 2 bước
4. Tiếp tục quá trình này cho đến khi tất cả các đỉnh đều được thăm

### Cài đặt:

- Sử dụng hàng đợi (queue)

### Ứng dụng:

- Tìm đường đi ngắn nhất trong đồ thị không có trọng số
- Thuật toán tìm kiếm từ điển
- Tìm tất cả các nút trong một thành phần liên thông
- Kiểm tra tính chất hai phía (bipartite) của đồ thị