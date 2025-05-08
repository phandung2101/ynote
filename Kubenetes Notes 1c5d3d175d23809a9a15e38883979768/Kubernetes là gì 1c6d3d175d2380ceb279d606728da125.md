# Kubernetes là gì?

# Ý tưởng

Sinh ra để giải quyết các bài toán về

- High **Availability** or no downtime
- **Scalability** or high performance
- **Disaster recovery** - backup and restore

# Architecture

![image.png](Kubernetes%20la%CC%80%20gi%CC%80%201c6d3d175d2380ceb279d606728da125/image.png)

Bao gồm

- Control Plan ( Master node )
    - API Server
        - entrypoint to K8S cluster
        - các thao tác như UI, API, CLI đều phải thông qua API Server
    - Controller Manager:
        - tracking trạng thái của cluster
    - Scheduler:
        - đảm bảo Pods hoạt động đúng cách
        - là nơi quyết định xem Pod sẽ được đặt ở Node nào
    - etcd
        - kubernetes backing store
        - lưu trữ thông tin cấu hình, snapshot, …
- Node ( Worker Node )
- Virtual Network
    - tạo ra 1 network thống nhất dùng để giao tiếp