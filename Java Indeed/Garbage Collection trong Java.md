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