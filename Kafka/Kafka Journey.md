# Kafka là gì?
**Apache Kafka** là một nền tảng _streaming_ dữ liệu phân tán, dùng để:
- Truyền tải dữ liệu giữa các hệ thống (như microservices).
- Lưu trữ dữ liệu theo thời gian thực.
- Xử lý dòng dữ liệu liên tục (stream processing).
Nó được thiết kế để _xử lý hàng triệu message mỗi giây_ với độ trễ thấp.

# Kiến trúc cơ bản
Gồm 4 thành phần chính sau đây:
- **Producer**: Gửi dữ liệu vào Kafka (ví dụ: gửi log, đơn hàng mới, etc).
- **Consumer**: Đọc dữ liệu từ Kafka để xử lý (ví dụ: lưu DB, gửi mail, etc).
- **Broker**: Là node chạy Kafka. Một cluster Kafka có thể có nhiều broker.
- **Topic**: Dữ liệu trong kafka được phân loại theo các chủ đề gọi là "topic"

# Cách streaming hoạt động
1. Producer gửi message vào topic
2. Kafka lưu trữ message đó (theo thứ tự, giống như file log)
3. Consumer đăng ký đọc topic đó
4. Kafka gửi topic đó cho consumer

