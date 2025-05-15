Là quá trình tự động quản lý tiến trình của memory giúp Java chạy hiệu quả. Java compile thành bytecode để có thể chạy trên JVM. Object được tạo trong heap memory, thứ là 1 phần bộ nhớ dành riêng cho chương trình. Sau cùng thì 1 số object sẽ không cần nữa. Garbage collector tìm những object không được sử dụng và xóa chúng khỏi bộ nhớ
# Làm việc với Garbage Collection
- Là một tiến trình tự động nhằm quản lý memory trên heap
- Nó xác định object nào vẫn đang sử dụng (referenced) và object nào không được sử dụng (unreferenced)
- nó xóa object thứ mà không được tham chiếu tới nữa
- programmer không phải đánh dấu rõ ràng để xóa những object này
# Các loại Garbage Collection
1. **Minor** hay **Incremental GC**: những object không được tham chiếu nữa trên **Young Generation heap memory** bị xóa.
2. **Major** hay **Full GC**: những object còn tồn tại qua **Minor GC** bị xóa khỏi **Old Generation heap memory**. Ít khi xảy ra hơn **Minor GC**
# Giải thích sơ qua về các vùng nhớ trong Heap memory
## Young Generation
Là mới được tạo ra sẽ được lưu trữ đầu tiên. Vì phần lớn object trong Java có "vòng đời ngắn" nên vùng này được tạo ra để **thu gom rác nhanh chóng hơn**.
### Cấu trúc bao gồm
- Eden Space: Nơi đầu tiên object tạo ra đưa vào
- Survivor Space S0
- Survivor Space S1
### Cách hoạt động
- Khi object được tạo ra -> nó vào vùng Eden
- Khi xảy ra Minor GC:
	- Các Object còn sống sẽ vào Survivor Space
	- Các Object chết sẽ được thu gom
- Các object sống sót nhiều lần sẽ được đẩy sang Old Gen
> Lưu ý: Khi Minor GC chạy thì dừng tất cả chương trình trong thời gian rất ngắn còn gọi là **stop-the-world**, minor GC xảy ra thường xuyên và liên tục
## Old Generation
Chứa các object có vòng đời dài hơn sống sót qua nhiều lần GC ở Young Gen. Vì vậy vùng này ít khi chạy GC và mỗi lần chạy sẽ tốn thời gian và tài nguyên hơn minor GC
### Cách hoạt động
- Khi object sống qua nhiều lần (chẳng hạn 15 lần) minor GC thì sẽ được đẩy qua Old Gen.
- Khi Old Gen đầy thì **Major GC** sẽ chạy.
> Lưu ý: Lưu ý **Major GC/Full GC** chạy sẽ tốn tài nguyên và chậm nên sẽ ảnh hưởng hệ thống. Mỗi khi chạy hệ thống sẽ pause lớn.
# Những điểm chính của GC
## 1. Unreachable Objects
Một object trở nên unreachable khi nó không còn được tham chiếu nữa. 
## 2. Khi nào Object đạt tiêu chuẩn để GC
Có nhiều cách để làm object unreachable:
- Thay đổi giá trị biến
- Trỏ sang giá trị null
- Object được tạo ra trong method block
- Island of isolation (object bị cô lập và không được cái nào tham chiếu đến nữa)
> Chúng ta có thể chủ động chạy GC bằng cách sử dụng `System.gc()` hoặc `Runtime.getRuntime().gc()`. Nhưng việc này không có nghĩa nó chạy lập tức mà chỉ đơn giản là sẽ chạy lúc nào đấy.
## 3. Finalization
Trước khi thực hiện destroy object, GC sẽ gọi `finalize()` để thực hiện cleanup activity. 
- finalize() method được gọi bởi GC chứ không phải JVM
- default implment của finalize() là empty
- được gọi mỗi lần với 1 object
- exception throw ra ở đây nếu không catch thì sẽ bị ignore và dừng finalization.
