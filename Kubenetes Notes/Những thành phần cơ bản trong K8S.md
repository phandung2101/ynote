# Những thành phần cơ bản trong K8S

# 1. Namespaces

Namespaces là cách Kubernetes phân chia tài nguyên trong một cụm thành các nhóm logic riêng biệt. Chúng giúp:

- Cô lập tài nguyên giữa các dự án, nhóm, hoặc môi trường khác nhau (dev, staging, production)
- Quản lý quyền truy cập và phân quyền
- Ngăn ngừa xung đột tên giữa các ứng dụng
- Giới hạn tài nguyên cho mỗi không gian tên

Ví dụ: `default`, `kube-system`, hay namespaces do người dùng tạo như `my-application`.

# 2. Nodes

Nodes là các máy vật lý hoặc máy ảo chạy các ứng dụng được Kubernetes quản lý. Mỗi node:

- Chạy các container của các ứng dụng
- Có kubelet chịu trách nhiệm giao tiếp với control plane
- Có container runtime (như Docker, containerd) để chạy các container
- Bao gồm các thành phần như kube-proxy để quản lý mạng

Nodes được phân loại thành worker nodes (chạy workloads) và master nodes (chạy control plane).

# 3. Workloads

Workloads là các ứng dụng chạy trên Kubernetes, bao gồm:

- **Pod**: Đơn vị nhỏ nhất, chứa một hoặc nhiều container
- **Deployment**: Quản lý việc triển khai và cập nhật các pods
- **StatefulSet**: Quản lý các ứng dụng có trạng thái
- **DaemonSet**: Đảm bảo mỗi node chạy một instance của pod
- **Job/CronJob**: Chạy các tác vụ theo lịch hoặc một lần
- **ReplicaSet**: Đảm bảo số lượng pod đang chạy

# 4. Network

Network trong K8S bao gồm:

- **Services**: Cung cấp địa chỉ IP và DNS cố định cho một nhóm pods
- **Ingress**: Quản lý truy cập bên ngoài đến services trong cluster
- **NetworkPolicy**: Quản lý luồng mạng giữa các pods
- **Service Mesh**: Quản lý giao tiếp giữa các microservices (Istio, Linkerd)
- **CNI (Container Network Interface)**: Plugin mạng (Calico, Flannel, Cilium)

Kubernetes sử dụng mô hình mạng flat, mọi pod có thể giao tiếp với nhau mà không cần NAT.

# 5. Storage

Storage trong K8S bao gồm:

- **PersistentVolume (PV)**: Tài nguyên lưu trữ trong cluster
- **PersistentVolumeClaim (PVC)**: Yêu cầu lưu trữ của người dùng
- **StorageClass**: Định nghĩa các loại storage với các thuộc tính khác nhau
- **ConfigMap và Secret**: Lưu trữ cấu hình và dữ liệu nhạy cảm
- **Volume**: Lưu trữ tạm thời hoặc vĩnh viễn gắn với lifecycle của pod

# 6. Configuration

Configuration bao gồm:

- **ConfigMaps**: Lưu trữ dữ liệu cấu hình không nhạy cảm
- **Secrets**: Lưu trữ dữ liệu nhạy cảm (passwords, tokens, keys)
- **ResourceQuotas**: Giới hạn tài nguyên cho namespaces
- **LimitRanges**: Đặt giới hạn mặc định cho containers
- **HorizontalPodAutoscaler**: Tự động mở rộng pods dựa trên mức sử dụng

# 7. Custom Resource

Custom Resource cho phép mở rộng Kubernetes API với tài nguyên mới:

- **Custom Resource Definition (CRD)**: Định nghĩa tài nguyên tùy chỉnh
- **Custom Controllers**: Logic xử lý cho tài nguyên tùy chỉnh
- **Operator Pattern**: Kết hợp CRD và Controller để tự động hóa các tác vụ phức tạp

# 8. Helm Releases

Helm là một package manager cho K8S:

- **Chart**: Gói các tài nguyên K8S liên quan
- **Release**: Instance của một chart triển khai trong cluster
- **Repository**: Nơi lưu trữ và chia sẻ charts
- **Values**: Cấu hình tùy chỉnh cho chart khi triển khai

Helm Releases giúp quản lý việc cài đặt, nâng cấp và rollback các ứng dụng phức tạp trên Kubernetes.