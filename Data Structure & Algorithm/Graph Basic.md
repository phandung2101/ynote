## 1. Khái niệm đồ thị (Graph)

Đồ thị là cấu trúc dữ liệu phi tuyến tính bao gồm:

- **Đỉnh (Vertices/Nodes)**: Các điểm trong đồ thị
- **Cạnh (Edges)**: Các đường nối giữa các đỉnh

Biểu diễn toán học: G(V, E) - trong đó V là tập các đỉnh và E là tập các cạnh.

## 2. Phân loại đồ thị

- **Đồ thị vô hướng (Undirected Graph)**: Cạnh nối giữa 2 đỉnh không có hướng
- **Đồ thị có hướng (Directed Graph)**: Cạnh nối có hướng từ đỉnh này đến đỉnh khác
- **Đồ thị có trọng số (Weighted Graph)**: Mỗi cạnh có một giá trị/trọng số
- **Đồ thị không trọng số (Unweighted Graph)**: Cạnh không có trọng số

## 3. Cách biểu diễn đồ thị

### 3.1. Ma trận kề (Adjacency Matrix)

Ma trận kề là ma trận vuông kích thước n×n (n là số đỉnh):

- Phần tử adjMatrix[i][j] = 1 nếu có cạnh từ đỉnh i đến đỉnh j
- Phần tử adjMatrix[i][j] = 0 nếu không có cạnh

**Ví dụ (Java):**

```Java
// Khởi tạo ma trận kề cho đồ thị 4 đỉnh
int V = 4;
int[][] adjMatrix = new int[V][V];

// Thêm cạnh vào đồ thị vô hướng
void addEdge(int i, int j) {
    adjMatrix[i][j] = 1;
    adjMatrix[j][i] = 1; // Đối với đồ thị vô hướng
}

// Thêm cạnh vào đồ thị có hướng
void addDirectedEdge(int i, int j) {
    adjMatrix[i][j] = 1;
}
```

**Ưu điểm:**

- Kiểm tra tồn tại cạnh nhanh: O(1)
- Dễ cài đặt

**Nhược điểm:**

- Tốn không gian lưu trữ: O(V²)
- Không hiệu quả với đồ thị thưa (sparse graph)

### 3.2. Danh sách kề (Adjacency List)

Mảng các danh sách liên kết, mỗi danh sách tại vị trí i chứa các đỉnh kề với đỉnh i.

**Ví dụ (Java):**

```Java
// Khởi tạo danh sách kề
int V = 4;
ArrayList<Integer>[] adjList = new ArrayList[V];
for (int i = 0; i < V; i++) {
    adjList[i] = new ArrayList<Integer>();
}

// Thêm cạnh vào đồ thị vô hướng
void addEdge(int i, int j) {
    adjList[i].add(j);
    adjList[j].add(i); // Đối với đồ thị vô hướng
}

// Thêm cạnh vào đồ thị có hướng
void addDirectedEdge(int i, int j) {
    adjList[i].add(j);
}
```

**Ưu điểm:**

- Tiết kiệm không gian lưu trữ: O(V + E)
- Hiệu quả khi duyệt qua tất cả các đỉnh kề của một đỉnh
- Phù hợp cho đồ thị thưa

**Nhược điểm:**

- Kiểm tra tồn tại cạnh chậm hơn: O(degree(V))

## 4. So sánh hai cách biểu diễn

|   |   |   |
|---|---|---|
|Tiêu chí|Ma trận kề|Danh sách kề|
|Không gian lưu trữ|O(V²)|O(V + E)|
|Kiểm tra cạnh|O(1)|O(degree(V))|
|Duyệt đỉnh kề|O(V)|O(degree(V))|
|Thích hợp|Đồ thị dày (dense)|Đồ thị thưa (sparse)|

## 5. Lựa chọn cách biểu diễn

- **Sử dụng ma trận kề khi**:
    - Đồ thị có nhiều cạnh (đồ thị dày)
    - Cần kiểm tra nhanh sự tồn tại của cạnh
    - Số lượng đỉnh ít
- **Sử dụng danh sách kề khi**:
    - Đồ thị có ít cạnh (đồ thị thưa)
    - Cần tiết kiệm không gian bộ nhớ
    - Thuật toán yêu cầu duyệt các đỉnh kề thường xuyên