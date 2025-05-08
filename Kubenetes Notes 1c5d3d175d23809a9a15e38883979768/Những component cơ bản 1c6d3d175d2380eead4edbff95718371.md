# Những component cơ bản

# Node

Hay hiểu đơn giản là máy ảo hoặc máy vật lý nơi chạy K8S Node

# Pod

Pod là đơn vị nhỏ nhất và cơ bản nhất trong hệ sinh thái Kubernetes. Nó đại diện cho một nhóm một hoặc nhiều container được triển khai và quản lý cùng nhau trên cùng một không gian mạng và không gian lưu trữ.

## Đặc điểm chính của Pod

- Đơn vị nhỏ nhất trong K8S
- Abstraction over container - Hiểu nôm na là cung cấp interface để giao tiếp với container runtime cụ thể như Docker hay containerd …. mà không cần quan tâm cách thao tác với Docker hay container runtime cụ thể nào
- Thường chỉ 1 App mỗi pod
- Mỗi Pod có 1 IP

## Điểm cần lưu ý

- mỗi khi Pod bị terminate hoặc restart thì sẽ có 1 IP mới được gắn vào ⇒ vậy nên để đảm bảo việc kết nối chúng ta cần sử dụng `Service`

# Service

- Permanent IP address
- Vòng đời của Service và Pod khác nhau ví dụ như kể cả Pod có die thì Service vẫn tồn tại nên đảm bảo cho việc có Static IP address
- Có 2 loại External Service và Internal Service
    - External service sử dụng ip của node cùng với port ví dụ `node-ip:port` ip này có thể truy cập từ ngoài
    - Internal service sử dụng DNS nội bộ k8s network để phân giải → ví dụ như dùng `db-service-ip:port`
    - Điển hình nhất application có thể dùng external service nhưng internal thì không.
- Cũng chính là 1 load balancer

# Ingress

- Trên thực tế chúng ta sẽ cần sử dụng https hay domain name thay vì ip. Vì thế chúng ta cần 1 component khác ở đây là Ingress
- Ingress sẽ foward traffic tới service

# Config Map

- Ví dụ khi có cập nhật về ENV_VARIABLE chúng ta thường sẽ phải rebuild lại container ( build image mới, push image lên repo, pull image về pod, … ) ⇒ khá là nhiều bước
- Thay vì thế chúng ta sử dụng ConfigMap nơi lưu trữ variables theo dạng key-value.
- Mỗi lần có cập nhật chúng ta chỉ cần update lại ConfigMap thì pod tự lấy thông tin mới
- Chỉ dùng lưu trữ những thông tin ít bảo mật. Vì nội dung sẽ được lưu dưới dạng plaintext

# Secret

- lưu trữ secret data
- liên kết secret trong Deployment/Pod

# Volume

- nơi lưu trữ data ví dụ cho database
- có thể lưu local hoặc remote storage
- k8s không đảm bảo việc lưu trữ data lâu dài ⇒ admin phải tự cấu hình cho việc backup hoặc lưu trữ

# Deployment

- Blueprint của 1 hoặc nhiều Pods
- Tương tự như Pod là abstraction của container, chúng ta có Deployment là abstraction của Pods
- Nơi khai báo replication config
- Không thể dùng cho database vì thực tế database cần phải lưu trữ quản lý data có tuần tự trong khi deployment có triển khai hoặc replica nhiều Pod cùng lúc ⇒ có thể làm data không nhất quán ⇒ thay vì thế sử dụng StatefulSet

# StatefulSet

- sử dụng cho Stateful Apps hoặc Databases
- kể cả việc sử dụng StatefulSet cũng khá khó ⇒ nên thường database sẽ thường được đặt riêng khỏi Kubenetes cluster