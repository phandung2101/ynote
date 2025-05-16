[![](https://media.geeksforgeeks.org/wp-content/uploads/20240424125622/Introduction-to-tree-.webp)](https://media.geeksforgeeks.org/wp-content/uploads/20240424125622/Introduction-to-tree-.webp)

## Cấu trúc dữ liệu Tree (Cây)

### 1. Định nghĩa

- **Tree** là cấu trúc dữ liệu phi tuyến tính (non-linear), có tổ chức phân cấp (hierarchical)
- Bao gồm các **node** (nút) được kết nối với nhau bởi các **edge** (cạnh)
- Có một node đặc biệt gọi là **root** (gốc), là node duy nhất không có parent (cha)
- Mỗi node có thể có nhiều child node (node con), tạo thành cấu trúc đệ quy

### 2. Thuật ngữ quan trọng

- **Node**: Đơn vị cơ bản của cây, chứa dữ liệu và tham chiếu đến các node con
- **Edge**: Liên kết giữa hai node, mỗi cây với n node sẽ có (n-1) edge
- **Root Node**: Node gốc, không có parent, là điểm bắt đầu của cây
- **Parent Node**: Node cha của một node khác
- **Child Node**: Node con của một node khác
- **Sibling Nodes**: Các node con cùng cha
- **Leaf Node/External Node**: Node lá, không có node con nào
- **Internal Node**: Node có ít nhất một node con
- **Ancestor**: Bất kỳ node nào trên đường đi từ root đến node hiện tại
- **Descendant**: Bất kỳ node nào trên đường đi từ node hiện tại đến các leaf node
- **Level của Node**: Số lượng edge từ root đến node đó (root có level 0)
- **Height của Node**: Đường đi dài nhất từ node đó đến một leaf node
- **Height của Tree**: Đường đi dài nhất từ root đến một leaf node
- **Depth của Node**: Số lượng edge từ root đến node đó
- **Degree của Node**: Số lượng node con trực tiếp của node đó
- **Subtree**: Một node cùng với tất cả descendant của nó

[![](https://media.geeksforgeeks.org/wp-content/uploads/20250214120937877633/treeTerminologies.webp)](https://media.geeksforgeeks.org/wp-content/uploads/20250214120937877633/treeTerminologies.webp)

### 3. Tại sao Tree được coi là cấu trúc dữ liệu phi tuyến tính?

- Dữ liệu trong cây không được lưu trữ theo cách tuần tự
- Dữ liệu được sắp xếp theo nhiều cấp độ (hierarchical structure)
- Mỗi phần tử có thể kết nối với nhiều phần tử khác

### 4. Cài đặt đơn giản của Node trong Tree

```Java
class TreeNode {
    int data;
    List<TreeNode> children;

    public TreeNode(int data) {
        this.data = data;
        this.children = new ArrayList<>();
    }

    public void addChild(TreeNode child) {
        children.add(child);
    }
}
```

### 5. Các loại cây

- **Binary Tree**: Mỗi node có tối đa 2 node con
- **Ternary Tree**: Mỗi node có tối đa 3 node con
- **N-ary Tree/Generic Tree**: Mỗi node có thể có nhiều node con

### 6. Các thao tác cơ bản

- **Create**: Tạo cây mới
- **Insert**: Thêm node vào cây
- **Search**: Tìm kiếm một node trong cây

### 7. Ứng dụng thực tế

- Cấu trúc thư mục trong hệ điều hành
- Cấu trúc thẻ trong HTML hoặc XML
- Cơ sở dữ liệu phân cấp
- Cây quyết định trong AI
- Cấu trúc tổ chức trong một công ty