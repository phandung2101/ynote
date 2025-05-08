# Kubernetes Configuration

# Cấu trúc

Bao gồm 3 phần chính

- metadata
- specification
- status (sẽ được K8S tự điều chỉnh)

# K8S lấy status data như thế nào?

- phase quan trọng để quyết định việc triển khai các config
- `etcd` giữ trạng thái của bất cứ k8s component nào
- lấy thông tin từ trong này thông qua control plane

# Minikube?

- single node cluster k8s
- very small
- fit for local test

# Kubectl

- Command line interface
- Nhiều chức năng nhất so với API và UI
-