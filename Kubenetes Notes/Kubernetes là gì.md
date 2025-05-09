# Ý tưởng

Sinh ra để giải quyết các bài toán về

- High **Availability** or no downtime
- **Scalability** or high performance
- **Disaster recovery** - backup and restore

# Architecture

![image.png](Kubenetes%20Notes/image.png)

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