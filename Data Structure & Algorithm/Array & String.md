# Array

## Định nghĩa:

- Là cấu trúc dữ liệu lưu trữ các phần tử cùng kiểu dữ liệu
- Các phần tử được lưu trữ liên tiếp trong bộ nhớ
- Có kích thước cố định (với array truyền thống)

## Cách khai báo trong Java:

```Java

// Khai báo và khởi tạo
int[] numbers = new int[5];// Mảng 5 số nguyên
int[] numbers = {1, 2, 3, 4, 5};// Khởi tạo trực tiếp// Mảng 2 chiều
int[][] matrix = new int[3][3];// Ma trận 3x3
```

## Thao tác cơ bản

### Truy cập phần tử

```Java
int firstElement = numbers[0];// Lấy phần tử đầu tiên
numbers[0] = 10;// Gán giá trị
```

### Duyệt mảng:

```Java
// Cách 1: For truyền thống
for(int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}

// Cách 2: For-each
for(int num : numbers) {
    System.out.println(num);
}
```

# String

## Định nghĩa:

- Là chuỗi các ký tự
- Trong Java, String là immutable (không thể thay đổi)
- Có nhiều phương thức hỗ trợ xử lý chuỗi

## Khai báo:

```Java
String str1 = "Hello";// Cách 1
String str2 = new String("Hello");// Cách 2
```

## Các thao tác cơ bản:

### Nối chuỗi:

```Java
String s1 = "Hello";
String s2 = "World";
String s3 = s1 + " " + s2;// "Hello World"// Hoặc
String s4 = s1.concat(" ").concat(s2);
```

### So sánh chuỗi:

```Java
// So sánh nội dung
boolean isEqual = str1.equals(str2);

// So sánh không phân biệt hoa thường
boolean isEqualIgnoreCase = str1.equalsIgnoreCase(str2);
```

### Các phương thức hữu ích:

```Java
String s = "Hello World";
int length = s.length();// Độ dài chuỗi
char c = s.charAt(0);// Lấy ký tự tại vị trí
String sub = s.substring(0, 5);// Cắt chuỗi
boolean starts = s.startsWith("Hello");// Kiểm tra bắt đầu
String upper = s.toUpperCase();// Chuyển thành chữ hoa
String lower = s.toLowerCase();// Chuyển thành chữ thường
String trimmed = s.trim();// Xóa khoảng trắng đầu cuối
```

## Hiệu năng và Lưu ý:

1. Array:
    - Truy cập ngẫu nhiên nhanh O(1)
    - Thêm/xóa phần tử chậm O(n)
    - Kích thước cố định
    - Tiết kiệm bộ nhớ
2. String:
    - Immutable: tạo đối tượng mới khi thay đổi
    - Dùng StringBuilder cho nối chuỗi nhiều lần
    - Pool của String giúp tối ưu bộ nhớ
    - equals() vs == trong so sánh

## Ví dụ thực tế:

```Java
// StringBuilder để nối chuỗi hiệu quả
StringBuilder sb = new StringBuilder();
for(int i = 0; i < 1000; i++) {
    sb.append("Number ").append(i).append(" ");
}
String result = sb.toString();

// Xử lý mảng
int[] arr = {1, 2, 3, 4, 5};
Arrays.sort(arr);// Sắp xếp
int pos = Arrays.binarySearch(arr, 3);// Tìm kiếm nhị phân
```