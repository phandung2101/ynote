# Source:

1. [https://jwt.io/introduction](https://jwt.io/introduction)

# 1. JWT là gì?

- là 1 tiêu chuẩn (RFC 7519) sử dụng dưới dạng JSON có chữ ký số. JWT thường dùng sử dụng để xác thực và ủy quyền giữa 2 bên
- gồm 3 phần: header, payload, signature theo dạng như sau: `header.payload.signature`
- mỗi phần là 1 chuỗi mã hóa bằng Base64URL trừ signature được encrypt

## 1.1. Header

Bao gồm 2 thành phần quan trọng là loại token và thuật toán ký số:

```json
{
  "alg": "HS256", // SHA, RSA bla bla
  "typ": "JWT"
}

```

***Note***: thuật toán có 2 loại phổ biến là:

- **HSA (HMAC with SHA)** - đối xứng -> dùng 1 secret key sử dụng hash mức độ bảo mật không cao bằng RSA nhưng bù lại hiệu suất nhanh
- **RSA** - bất đối xứng -> dùng 1 cặp public-secret key mức độ bảo mật cao thường dùng trong hệ thống phân tán lớn nhưng bù lại do độ phức tạp cao nên việc encrypt, valid hay store tốn hiệu năng và tài nguyên

## 1.2. Payload

Payload chứa các thông tin (claims) mà bạn muốn chia sẻ. Có ba loại claims phổ biến:

- Registered Claims: Các claims được định nghĩa sẵn bởi chuẩn JWT:
    - `iss` (Issuer): Xác định nguồn tạo ra JWT.
    - `sub` (Subject): Xác định chủ sở hữu của JWT.
    - `aud` (Audience): Xác định bên nhận dự định của JWT.
    - `exp` (Expiration Time): Xác định thời gian hết hạn của JWT.
    - `nbf` (Not Before): Xác định thời điểm JWT trở nên hợp lệ.
    - `iat` (Issued At): Xác định thời điểm JWT được tạo ra.
    - `jti` (JWT ID): Xác định ID duy nhất cho JWT.
- Custom Claims: Do người dùng tự định nghĩa. Ví dụ:

```json
{ "user_id": "12345", "role": "admin", "email": "john@example.com" }

```

## 1.3. Signature

Chữ kí là phần quan trọng nhất trong JWT. Vậy signature được tạo ra như nào?

- gom dữ liệu từ `header + "." + payload`
- sử dụng thuật toán mã hóa ở header và secret key để tạo chữ kí

# 2. JWT process:

**JWT được ứng dụng trong việc authorization như thế nào?**

1. client sử dụng username/password để đăng nhập (authentication) ở **auth server** thành công sẽ trả về **access token**
2. client sử dụng **access token** để gắn kèm với request tới **resource server**
3. **Resource server** xác thực token có hợp lệ hay không (chữ ký, hạn sử dụng, ...) -> nếu hợp lệ thì thực thi tiếp và nếu không thì trả lỗi

**JWT - client side:**
Ở phía client side thì jwt token thường sẽ được lưu ở local storage, hoặc cookie. Có 1 số nhược điểm với cách này như sau:

- rủi ro về tấn công xss, cors, csrf
- khả năng token bị lộ
- khó kiểm soát và thu hồi token
- không thể chấm dứt phiên hoạt động
Để giảm thiểu có 1 số cách sau:
- Sử dụng HttpOnly và Secure flag cho cookie chứa token, giảm thiểu nguy cơ truy cập token qua JavaScript và đảm bảo giao tiếp qua HTTPS.
- Xác thực và xử lý CORS đúng cách để ngăn chặn việc truy cập token từ các nguồn không tin cậy.
- Sử dụng CSRF token để bảo vệ khỏi các tấn công CSRF.
- Xác thực và phê duyệt tất cả các yêu cầu từ phía server để đảm bảo tính toàn vẹn và xác thực của token.
- Sử dụng thời gian hết hạn (expiration time) hợp lý cho token và chấm dứt phiên hoạt động kịp thời khi cần thiết.

# 3. Access token & Refresh token

Việc sử dụng mỗi **access token** thường sẽ tiềm ẩn khá nhiều nguy cơ nên 1 cách thường sử dụng là dùng thêm **refresh token** và yêu cầu để áp dụng là:

- access token: exp time ngắn (thường từ 15p -> 1 ngày tuỳ mức độ sử dụng)
- refresh token: exp time dài (thường là khoảng 7 ngày)
- refresh token sẽ được dùng để "refresh" lại access token hay xin cấp mới access token

# 4. JWT Implementation

Việc implement jwt trong backend tuỳ thuộc khá nhiều vào nhiều yếu tố như hiệu năng cũng như mức độ bảo mật. Ví dụ với yêu cầu mức độ bảo mật cao thì sẽ phải đánh đổi bằng hiệu năng và ngược lại (*everything is a trade off*)

Sau khi tham khảo 1 số cách thì có 1 cách khá cân bằng được nhiều yếu tố. Với cách này vẫn sẽ đảm bảo được mức độ bảo mật và không phải đánh đổi quá nhiều hiệu năng và được sử dụng khá phổ biến

**Step by step:**

1. **User login** -> username/password login -> auth server -> cặp access/refresh token
    - access token: có thể lưu được bất kì đâu và thời gian expiry time thời sẽ ngắn (15m -> 1h)
    - refresh token: expiry time khoảng 7 ngày, cần lưu 1 cách bảo mật (secured, HttpOnly cookie)
2. **Store refresh token**:
    - lưu **refresh token** ở DB bên cạnh user id hoặc username, thông tin hết hạn **refresh token**, và các thông tin liên quan (device, ip, ...)
    - cần phải lưu để **validate** những token hết hạn
3. **User API call:**
    - sử dụng access token request tới resource server
    - resource validate access token theo thông tin hết hạn
4. **Access token expiration:**
    - nếu user sử dụng **access token** hết hạn thì server trả về 401
    - trong trường hợp này user phải sử dụng **refresh token** để lấy **access token** mới
5. **Refresh token API:**
    - validate token này đã hết hạn chưa -> nếu hết hạn thì return 401
    - backend phải find trong database về refresh token -> nếu không tồn tại hoặc hết hạn thì return 401 -> user sẽ phải authenticate lại
    - nếu refresh token valid thì sẽ tạo mới 1 cặp access/refresh token mới và set invalid cho refresh token cũ
6. **Logout**
    - với trường hợp user log out thì sẽ set invalid cho refresh token hiện tại
7. **Periodic Cleanup:**
- để tránh việc dữ liệu rác ở db quá nhiều thì nên sử dụng các phương án để xoá những refresh token quá hạn hoặc invalid ở DB

***Note:*** Lưu ý phương án trên chỉ là 1 số điểm lưu ý khi implement JWT. Ở phương án cụ thể nên tuỳ vào yêu cầu của project để đưa ra lựa chọn phù hợp và chính xác nhất.

# 5. Gây lú (1 số điểm dễ gây hiểu nhầm):

- Việc sử dụng access token không làm giảm nguy cơ tiềm ẩn so với phương án sử dụng session => nhưng việc **access token** là **stateless** nên có tính linh hoạt của nó trong các mô hình microservice thực sự có lợi hơn
- JWT token không phải được **encrypt** mà nó được **sign** (ký số). Việc này sẽ đảm bảo thông tin trong token không được chỉnh sửa vì nếu có chỉnh sửa sẽ gây invalid ở các resource server nắm giữ **secret key**
- Việc dùng **refresh token** có thể gây hiểu nhầm vì nếu sử dụng sai thì refresh token chỉ có vai trò như 1 **access token** thứ 2 với expiry time dài hơn mà thôi
- Ưu điểm của **refresh token** thường được sử dụng trong mô hình **Identify Provider (Auth Server)** và **Resource Server** hay còn sử dụng trong các mô hình microservice. **Access token** sẽ được lưu local storage -> giúp cho việc sử dụng cho nhiều origin tiện lợi nhưng **Refresh Token** thì phải được lưu ở cookie cho **1 domain duy nhất là Identify Provider**