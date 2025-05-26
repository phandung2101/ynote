# Example

Khi số lượng request vượt quá khả năng chịu tải của web server, cụ thể ví dụ sau:

1. Service A gửi request tới B và C.
2. Trong đấy service B phải gọi external service D khác và response về A chậm
3. Lưu lượng request gửi tới A nhiều hơn tốc độ response của B cho A
4. Dần dần thread của service A sẽ cạn kiệt và service A sẽ sập
5. Trong trường hợp tệ hơn ví dụ service D không phản hồi và service B liên tục gọi tới D => dẫn tới service B sẽ sớm sập theo D và tới A.

Vậy để đảm bảo các service sẽ không phụ thuộc hoàn toàn và tránh hiện tượng sập hàng loạt chúng ta sử dụng **Circuit Breaker Pattern*** (nói cách đơn giản là cầu giao)

- phát hiện khi nào service chậm hoặc sập hẳn
- dùng biện pháp tạm thời để tránh tình huống trở nên tệ hơn
- tắt tạm thời phần lỗi để tránh ảnh hưởng các thành phần khác trong hệ thống

# Circuit Breaker Pattern

## When does the circuit trip?

- kiểm tra **n** request cuối để đưa ra quyết định
- bao nhiêu trên số đấy fail
- thời gian timeout

## When does the circuit un-trip?

- sau bao lâu thì circuit trip chạy lại

## Fallback

- trả về fail response (không phải phương án tốt nhưng là phương án tạm thời okela)
- "default" response ( giá trị mặc định cho request thay vì fail nhưng thật sự chưa phải lựa chọn tốt nhất)
- previous responses (cache) (trong 1 số hệ thống user khó có thể nhận biết được đâu là response đúng và sai) => tùy từng trường hợp và api để sử dụng
- Framework that support: Hystrix, spring-cloud-circuitbreaker, resilience4j

# Bulkhead Pattern

Tưởng tượng cả hệ thống là chiếc thuyền thì chúng ta cần tạo ra nhiều căn phòng. Nếu trường hợp có sự cố ở đâu thì chúng ta phải đóng khóa kín cửa phòng đó hoặc tầng hoặc ... để tránh cả chiếc thuyền chìm.