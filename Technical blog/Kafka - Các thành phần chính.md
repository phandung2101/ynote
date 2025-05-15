# Kafka là gì?
**Apache Kafka** là một nền tảng _streaming_ dữ liệu phân tán, dùng để:
- Truyền tải dữ liệu giữa các hệ thống (như microservices).
- Lưu trữ dữ liệu theo thời gian thực.
- Xử lý dòng dữ liệu liên tục (stream processing).
Nó được thiết kế để _xử lý hàng triệu message mỗi giây_ với độ trễ thấp.

## Kiến trúc cơ bản
Gồm 4 thành phần chính sau đây:
- **Producer**: Gửi dữ liệu vào Kafka (ví dụ: gửi log, đơn hàng mới, etc).
- **Consumer**: Đọc dữ liệu từ Kafka để xử lý (ví dụ: lưu DB, gửi mail, etc).
- **Broker**: Là node chạy Kafka. Một cluster Kafka có thể có nhiều broker.
- **Topic**: Dữ liệu trong kafka được phân loại theo các chủ đề gọi là "topic"

## Cách streaming hoạt động
1. Producer gửi message vào topic
2. Kafka lưu trữ message đó (theo thứ tự, giống như file log)
3. Consumer đăng ký đọc topic đó
4. Kafka gửi topic đó cho consumer

## Kafka topic, partition và offset
- **topic** là **stream of data**.
- topic có thể được chia thành một hoặc nhiều **partition** và message được lưu trên đó.
- **partition** được order và đánh dấu từ 0
- các message được lưu trên mỗi partition được order và append liên tục theo thứ tự là **offset** bắt đầu từ 0
## Broker
- topic được lưu trên disk để đảm bảo durability và nơi giữ disk đó là server hay gọi là **Kafka Broker**
- số lượng broker thường là 3,5,7,... mỗi broker được đánh id là integer
- lưu trữ 1 hoặc vài partition của topic chứ không phải tất cả
## Cách phân bổ topic trên broker
- cluster có xu hướng phân chia partition của topic ra nhiều broker nhất có thể
- xu hướng thường là phân bổ đều partition và đảm bảo số lượng partition trên mỗi broker cân đối
## Topic replication
- nếu chỉ phân bổ không thôi thì chưa giải quyết được single-point-failure thì vẫn dựa trên ý tưởng tạo nhiều bản sao cho partition => cơ chế này gọi là replication
- default replication factor = 1 => nếu tăng nó lên 3 thì chúng ta sẽ có mỗi partition được replica ra làm 3 bản
# Gửi và nhận message trong Kafka
## Leader partition concept
Như phần trên đã hiểu được rằng mỗi partition có thể có nhiều replication. Thế khi message gửi đến nó sẽ vào đâu. Thông thường với những bài toán này sẽ có 2 cáhc giải:
- peer-to-peer nghĩa là cùng vote 
- leader-follower nghĩa là chọn ra 1 leader quyết định tất
=> Kafka sử dụng cơ chế leader-follower lựa chọn 1 replication làm leader, tất cả **read/write message** đến sẽ được ghi vào **partition** này. Các replication khác còn gọi là **ISR (in-sync replica)**
## Producer
- Producer là nơi gửi message đến cho broker.
- Producer sẽ tự động biết write vào broker nào và partition nào (có sẵn cơ chế retry phòng trường hợp broker lỗi)
- Producer có sử dụng **ack - acknowledgment** để biết trạng thái message gửi đi
	- acks=0 - fire-and-forget 
	- acks=1 - (default) Replication Leader write thành công báo lại
	- acks=all - đảm bảo không mất vì chỉ thành công khi relication leader và ISR write data thành công
## Message key
Có 3 cách chỉ định partition đến của message:
1. Thêm key cho message. default kafka sử dụng **hash key partitioning** để tìm partition cho message => đảm bảo cùng key sẽ cùng partition
2. Hardcode. Chỉ định partition cho message => ít khi sử dụng dễ gây mất cân bằng
3. Tự define cơ chế partitioning, routing message bằng cách implement **Practitioner**
Nếu key = null thì Kafka sẽ tự sử dụng cơ chế **round-robin** để phân bổ gửi đến các partition.
Thường người ta chỉ quan tâm trên cùng 1 đơn vị được sắp xếp tới trước. Ví dụ tọa độ GPS của tài xế A liên tục được gửi đến chỉ cần ordering diễn ra trên tài xế này. Vậy chỉ cần sử dụng id của tài xế A làm key thì tất cả message của A đều vào cùng partition
## Consumer
### Consumer
- nơi đọc message từ topic xác định bằng topic name
- consumer có cơ chế phục hồi khi broker lỗi
- đọc message trong partition diễn ra tuần tự theo **offset**
- consumer có thể đọc từ 1 hoặc nhiều **partition** của topic
### Consumer Group
- khi số lượng producer tăng lên thì cũng cần số lượng consumer tương ứng. Các consumer này thường được gom vào nhóm gọi là **consumer group**
- mỗi consumer thuộc consumer group sẽ đọc 1 hoặc nhiều partition để đảm bảo ordering -> nhưng cùng group không tồn tại nhiều consumer đọc cùng partition 
- một partition không thể gửi cho nhiều consumer thuộc cùng 1 consumer group
- số lượng consumer trong group lớn hơn số lượng partition thì sẽ có nhiều consumer không nhận message gì và rơi vào trạng thái **inactive**
> Khi 1 active consumer bị lỗi thì 1 inactive consumer sẽ được đẩy lên thay thế. Nếu không có inactive consumer nào thì sẽ route đến active consumer khác. Quá trình re-assign này gọi là **partition rebalance**
## Queue và Topic trong Kafka
Cách chúng ta implement 2 cơ chế dưới trong Kafka như thế nào:
- Point-to-point hay là Queue
- Broadcast message hay là Topic

Sử dụng queue thì chỉ đơn giản là có 1 consumer group cho mỗi topic thôi thế là xong. Còn với broadcast thì nhân số lượng consumer group lên nhiều thêm

# Consumer Offset
Consumer offset giống như checkpoint hay bookmark trong consumer group. Mỗi group có offset khác nhau. Sau khi xử lý message thì offset sẽ được commit. Sẽ được lưu tại `__consumer_offsets` topic. Việc lưu này đảm bảo kể cả khi consumer đó down thì khi chạy tiếp tại consumer khác hay sau khi khởi động lại thì continue tại bước cuối cùng. 
Có 3 tình huống khi consumer commit offset:
## At most once
- Commit ngay khi nhận message 
- Điểm yếu nếu lỗi thì sẽ không retry trên message này được
## At least once
- Commit sau khi xử lý thành công
- Khắc phục được điểm yếu của **at most once** nhưng lại gặp vấn đề khác
- Điểm yếu là trong quá trình xử lý lỗi chưa commit được sẽ thực hiện lại. Nhưng việc thực hiện lại chính là vấn đề khi ví dụ trừ tiền 2 lần tài khoản chẳng hạn.
- Thường điểm yếu ở trên cần phải implement các cơ chế để process là idempotent. Tức là kể cả xử lý nhiều lần trên 1 input đều ra chung 1 kết quả.
- *Vì những điểm ở trên mà các hệ thống thường lựa chọn phương án này*.
## Exactly once
- Không áp dụng cho các consumer group dạng application như Java, C#, ...
- Chỉ sử dụng cho message transfer từ Kafka sang Kafka
- Hoặc chúng ta có thể implement được phương án này bằng cách sử dụng **at least once + idempotent pattern**
# Kafka Broker Discovery
Vì mỗi broker là 1 bootstrap server, nó có cơ chế thông báo cho client biết làm thế nào để kết nối tới các broker còn lại. Như vậy khi connect tới một broker là có thể biết cách connect tới các broker khác.
Mỗi broker có thông tin về broker, topic, partition của Kafka cluster, được gọi là metadata.
Khi client request tới có thể lấy metadata về cluster và truy cập đúng broker cần tìm.
# Zookeeper
Có nhiệm vụ:
- lưu trữ tất cả thông tin về Kafka cluster, topic,....
- thực hiện leader election cho partition
- gửi thông tin đến cluster về new topic, delete topic, parititon die, broker die, broker come up, ...
Các đặc điểm chính:
- Zookeeper triển khai theo cluster theo số lượng 3,5,7 ... nodes.
- Cũng tuân theo mô hình leader-follower leader thực hiện write, follower thực hiện read.