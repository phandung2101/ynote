### 1. Định nghĩa

- **Binary Search Tree** là một loại cây nhị phân đặc biệt
- Mỗi node có tối đa 2 node con (trái và phải)
- Với mọi node, giá trị của tất cả các node trong cây con bên trái < giá trị của node hiện tại, và giá trị của tất cả các node trong cây con bên phải > giá trị của node hiện tại

### 2. Đặc điểm

- Tìm kiếm, chèn, xóa đều có độ phức tạp trung bình là O(log n)
- Trong trường hợp xấu nhất (cây bị suy biến thành danh sách liên kết): O(n)
- Dữ liệu được lưu trữ theo thứ tự, giúp duyệt cây theo thứ tự tăng dần (inorder traversal)

### 3. Cài đặt Node trong BST

```Java
class BSTNode {
    int data;
    BSTNode left;
    BSTNode right;

    public BSTNode(int data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}
```

### 4. Các thao tác cơ bản trên BST

### a. Tìm kiếm

```Java
public BSTNode search(BSTNode root, int key) {
    // Trường hợp cơ sở: root rỗng hoặc key ở root
    if (root == null || root.data == key)
        return root;

    // Nếu key nhỏ hơn root, tìm kiếm bên trái
    if (root.data > key)
        return search(root.left, key);

    // Nếu key lớn hơn root, tìm kiếm bên phải
    return search(root.right, key);
}
```

### b. Chèn

```Java
public BSTNode insert(BSTNode root, int key) {
    // Nếu cây rỗng, tạo node mới
    if (root == null)
        return new BSTNode(key);

    // Nếu không, duyệt xuống cây
    if (key < root.data)
        root.left = insert(root.left, key);
    else if (key > root.data)
        root.right = insert(root.right, key);

    // Trả về con trỏ root (không thay đổi)
    return root;
}
```

### c. Duyệt cây (Inorder - trung thứ tự)

```Java
public void inorderTraversal(BSTNode root) {
    if (root != null) {
        inorderTraversal(root.left);    // Duyệt cây con trái
        System.out.print(root.data + " ");  // Thăm node hiện tại
        inorderTraversal(root.right);   // Duyệt cây con phải
    }
}
```

### d. Xóa node

```Java
public BSTNode deleteNode(BSTNode root, int key) {
    // Trường hợp cơ sở
    if (root == null) return null;

    // Tìm node cần xóa
    if (key < root.data)
        root.left = deleteNode(root.left, key);
    else if (key > root.data)
        root.right = deleteNode(root.right, key);
    else {
        // Node với 1 hoặc không có con
        if (root.left == null)
            return root.right;
        else if (root.right == null)
            return root.left;

        // Node với 2 con: Lấy node kế tiếp theo thứ tự (successor)
        root.data = minValue(root.right);

        // Xóa successor
        root.right = deleteNode(root.right, root.data);
    }
    return root;
}

// Tìm giá trị nhỏ nhất trong cây
private int minValue(BSTNode root) {
    int minValue = root.data;
    while (root.left != null) {
        minValue = root.left.data;
        root = root.left;
    }
    return minValue;
}
```

### 5. Khi nào BST bị suy biến thành danh sách liên kết?

BST bị suy biến thành danh sách liên kết khi mọi node chỉ có một node con, dẫn đến mất cân bằng. Điều này xảy ra khi:

- Dữ liệu đầu vào đã được sắp xếp (tăng dần hoặc giảm dần)
- Chèn liên tiếp các phần tử theo một thứ tự nhất định
- Ví dụ: Chèn 1,2,3,4,5 theo thứ tự sẽ tạo ra cây chỉ phát triển về bên phải

### 6. Ứng dụng của BST

- Từ điển, danh bạ
- Tìm kiếm dữ liệu nhanh chóng
- Priority Queue (Hàng đợi ưu tiên): Cấu trúc dữ liệu trong đó mỗi phần tử có độ ưu tiên và phần tử có độ ưu tiên cao nhất được xử lý trước
- Tổ chức dữ liệu theo thứ tự
- Làm nền tảng cho các cấu trúc dữ liệu cân bằng hơn như AVL, Red-Black Tree

### 7. Nhược điểm của BST

- Độ phức tạp phụ thuộc vào hình dạng của cây
- Cây có thể bị suy biến thành danh sách liên kết, làm giảm hiệu suất xuống O(n)
- Cần các giải thuật cân bằng cây để duy trì hiệu suất tối ưu