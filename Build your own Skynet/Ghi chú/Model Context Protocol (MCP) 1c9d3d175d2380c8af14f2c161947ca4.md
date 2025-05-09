# Model Context Protocol (MCP)

# Thông tin cơ bản

## Giới Thiệu

Model Context Protocol (MCP) là một tiêu chuẩn mới được phát triển nhằm tạo kết nối hai chiều hiệu quả giữa các Nguồn Dữ Liệu (Data Sources) và Công Cụ Trí Tuệ Nhân Tạo (AI-Powered Tools). Đây là cầu nối quan trọng giúp nâng cao khả năng tương tác giữa AI và các nền tảng ứng dụng.

## Thành Phần Cơ Bản

- **Data Sources**: Có thể là Hệ thống tệp, Kho lưu trữ GitHub, Google Drive và nhiều nguồn dữ liệu khác
- **AI-Powered Tools**: Bao gồm các mô hình AI như Claude, ChatGPT và các công cụ AI tiên tiến khác

## Ứng Dụng Thực Tế

MCP đặc biệt hữu ích trong lĩnh vực lập trình, nơi các Coding Agent như Zed, Replit, và Codeium có thể tích hợp protocol này để mô hình AI hiểu sâu hơn về ngữ cảnh của code. Bằng cách thu thập thêm thông tin ngữ cảnh, AI có thể đưa ra phản hồi chính xác và phù hợp hơn.

## Cơ Chế Hoạt Động

Các công cụ AI như Claude và ChatGPT vận hành dựa trên hai yếu tố chính:

### 1. Resource (Tài Nguyên)

- Chứa thông tin đầu vào cho AI (câu hỏi, văn bản, dữ liệu...)
- Mỗi resource bao gồm:
    - Mô tả nguồn dữ liệu
    - Định danh nguồn dữ liệu
    - Phương thức truy cập và thao tác

### 2. Tool (Công Cụ)

- Định nghĩa cách xử lý và tương tác với dữ liệu
- Mỗi tool bao gồm:
    - Mô tả chức năng
    - Cấu trúc dữ liệu đầu vào
    - Quy trình thực thi

MCP cung cấp một giao diện chung, tương tự như cách RestAPI sử dụng các phương thức GET, POST để thao tác với tài nguyên, giúp các ứng dụng dễ dàng tích hợp với các công cụ AI.

## Ví Dụ Minh Họa

Hãy xem xét một tình huống thực tế về việc đặt thuê nhà:

### Resource:

- **Mô tả**: "Danh sách giá nhà thuê Hà Nội"
- **Nguồn dữ liệu**: "Database bên thứ 3 có dữ liệu về giá nhà thuê Hà Nội"
- **Phương thức**: "Truy vấn toàn bộ thông tin giá nhà"

### Tool:

- **Mô tả**: "Đặt thuê nhà"
- **Cấu trúc đầu vào**: `{"id": "mã căn hộ", "start": "ngày bắt đầu", "end": "ngày kết thúc"}`
- **Quy trình**: "Gọi API đặt thuê dựa trên dữ liệu JSON đầu vào"

### Tương Tác Người Dùng:

1. **Người dùng**: "Đặt thuê tôi giá nhà rẻ nhất trong khoảng ngày 20/3 → 25/3"
2. **AI**: *(Tìm kiếm thông tin từ danh sách giá nhà thuê và xác định căn rẻ nhất trong khoảng thời gian yêu cầu)*
3. **AI**: "Tôi sẽ đặt phòng cho bạn nhà ABC từ 20-25/3. Bạn xác nhận điều này?"
4. **Người dùng**: "Tôi xác nhận"
5. **AI**: *(Sử dụng tool đặt thuê với thông tin đã xác nhận)*
6. **AI**: "Đặt phòng thành công. Thông tin chi tiết của bạn là..."

Thông qua Model Context Protocol, AI có thể liền mạch kết nối với các nguồn dữ liệu và công cụ khác, tạo ra trải nghiệm tương tác thông minh và hiệu quả cho người dùng.