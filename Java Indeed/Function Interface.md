## What

- là *interface có duy nhất 1 abstract method*, có thể không có hoặc có nhiều default/static method

```
@FunctionalInterface
public interface Flyable {
	void fly();
	default boolean alive() { return true; }
}

```

- annotation `@FunctionalInterface` là không bắt buộc nhưng để đánh dấu tránh sai sót
- còn được gọi là SAM type ( Signle abstract method ) vì đúng theo trên định nghĩa của nó

## Why

- trong các ngôn ngữ functional programming như js thì ==function là *first-class citizen*== nghĩa là function là số 1, ... có khả năng làm đối số function khác, làm giá trị trả về, hàm lồng trong hàm khác => linh hoạt và hỗ trợ tốt cho functional programing
- method trong java không linh hoạt như vậy do Java xem ==class là *first-class citizen*== chứ không phải function => do đó không thể thực hiện như js

```
function printResult(calculate) {
	console.log('Result is', calculate())
}

printResult(function () {
	return 3.14
})

```

=> Team Java đã nghĩ ra cách tiếp cận khác để giải quyết vấn đề dưới đây

## How

==Dùng functional interface để wrap method==

- java là strict, và mọi thứ phải rõ ràng ngược lại với js nên không thể sử dụng dynamic như javascript
- ví dụ sau mô tả cách sử dụng

```
// Định nghĩa khuôn mẫu cho method truyền vô
@FunctionalInterface
interface Calculable {
    double calculate();
}
...
// Có thể gọi được method trong khuôn mẫu
public void printResult(Calculable func) {
    System.out.println("Result: " + func.calculate());
}
...
// Method thực sự truyền vào, được wrap vô trong khuôn mẫu
printResult(new Calculable() {
    @Override
    public double calculate() {
        return 3.14;
    }
})

```