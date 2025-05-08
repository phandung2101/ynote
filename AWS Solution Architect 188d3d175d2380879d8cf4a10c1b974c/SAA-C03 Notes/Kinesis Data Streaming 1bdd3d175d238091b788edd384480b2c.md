# Kinesis Data Streaming

# Kiến thức cơ bản về Amazon Kinesis Data Streams

Amazon Kinesis Data Streams là dịch vụ xử lý dữ liệu theo thời gian thực cho phép thu thập và xử lý hàng loạt dữ liệu với độ trễ thấp. Dưới đây là những kiến thức cơ bản và thông số quan trọng:

## Khái niệm cơ bản

1. **Streams (Luồng dữ liệu)**: Đơn vị cơ bản trong Kinesis Data Streams, nơi lưu trữ dữ liệu.
2. **Shards (Phân mảnh)**: Đơn vị cơ bản của thông lượng trong một stream. Mỗi shard cung cấp khả năng nhận và gửi dữ liệu với công suất cụ thể.
3. **Records (Bản ghi)**: Đơn vị dữ liệu trong Kinesis. Mỗi record bao gồm:
    - Partition key (khóa phân vùng)
    - Sequence number (số thứ tự)
    - Data blob (dữ liệu)
4. **Producers (Nhà sản xuất)**: Ứng dụng gửi dữ liệu vào Kinesis Streams.
5. **Consumers (Người tiêu dùng)**: Ứng dụng xử lý dữ liệu từ Kinesis Streams.

## Thông số kỹ thuật quan trọng

1. **Thông số Shard**:
    - **Thông lượng ghi**: 1MB/giây hoặc 1,000 records/giây
    - **Thông lượng đọc**: 2MB/giây
    - **Số lượng shard tối đa**: 500 shard mặc định (có thể tăng theo yêu cầu)
2. **Thời gian lưu trữ dữ liệu**:
    - Mặc định: 24 giờ
    - Tối đa: 7 ngày
    - Với bản nâng cao: lên tới 365 ngày
3. **Kích thước dữ liệu**:
    - Kích thước record tối đa: 1MB
4. **PutRecords API**:
    - Tối đa 500 records mỗi lần gọi
    - Tối đa 5MB dữ liệu mỗi lần gọi
5. **Enhanced Fan-Out**:
    - 2MB/giây cho mỗi shard cho mỗi consumer
    - Tối đa 20 consumer đăng ký cho mỗi stream
6. **Mô hình tiêu thụ**:
    - **Shared throughput**: Nhiều consumer chia sẻ khả năng đọc 2MB/giây/shard
    - **Enhanced Fan-Out**: Mỗi consumer được cấp 2MB/giây/shard dành riêng

## Loại Consumer

1. **Kinesis Client Library (KCL)**: Thư viện giúp quản lý việc phân phối shard giữa nhiều consumer instances.
2. **AWS Lambda**: Tích hợp trực tiếp với Kinesis để xử lý dữ liệu.
3. **Kinesis Data Firehose**: Dịch vụ để chuyển dữ liệu từ Kinesis Streams đến điểm đích (S3, Redshift, Elasticsearch, Splunk).
4. **Kinesis Data Analytics**: Phân tích dữ liệu luồng theo thời gian thực.

## Độ trễ và hiệu suất

- Độ trễ thông thường: 200-300ms từ producer đến consumer
- Tính sẵn sàng cao với khả năng phục hồi trên nhiều vùng sẵn sàng

Đây là những kiến thức và thông số cơ bản về Amazon Kinesis Data Streams. Bạn có muốn tìm hiểu thêm về khía cạnh cụ thể nào không?​​​​​​​​​​​​​​​​