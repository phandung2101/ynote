# Helm là gì?

- là package manager dành cho K8S ( tương tự npm cho nodejs )

# Helm Chart là gì?

- là 1 bản đóng gói sẵn ( package yaml files ) chứa đầy đủ các secret, statefullset, … để có thể nhanh chóng cấu hình ứng dụng
- chúng ta có thể tự tạo helm chart từ bundle yml files của chúng ta rồi push lên helm repository
- hoặc đơn giản là download chart có sẵn và sử dụng tương tự như import package trong javascript

# Tại sao phải sử dụng?

- những công cụ như logging và monitoring hay database thường có tính chất tái sử dụng nếu phải tự cấu hình toàn bộ manifest file cho k8s thì rất phức tạp và tốn thời gian
- chúng ta có thể sử dụng Helm Chart có sẵn để deploy nhanh chóng hơn

# Helm Chart structure

- `chart.yaml` meta info about chart
- `values.yaml` values for the template file
- `charts` folder ( các dependencies )
- `templates`  folder ( nơi lưu trữ các file template files )

# Cách sử dụng template với value injection

Helm chart sẽ luôn có sẵn `values.yaml` chúng ta có thể khai báo file riêng `my-values.yaml` sau đó dùng command dưới đây để inject values

```bash
helm install --values=my-values.yaml <chartname>
```

Hoạt động theo cơ chế override ( tức những value mình không chủ động ghi đè sẽ sử dụng default value )

Hoặc sử dụng set command

```bash
helm install -set version=2.0.0
```

# Release Management

( chưa rõ lắm , tìm hiểu sau )