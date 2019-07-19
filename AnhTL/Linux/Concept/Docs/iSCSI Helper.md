# iSCSI Helper

**iSCSI helper hoạt động như thế nào**:
- iSCSI helper chỉ dẫn ta qua một loạt các bước để giúp ta cấu hình Target iSCSI (Server) hoặc initiator (client) của ta.
- Khi ta hoàn thành tất cả các bước, công cụ này sẽ tạo một tập lệnh dựa trên thông tin mà ta đã nhập
- Ta chạy tập lệnh này để chuẩn bị hệ thống cho vai trò được xác định là Target iSCSI (Server) hoặc initiator (client).

**Các tính năng của Công cụ iSCSI helper:**
- Ta có thể chỉ định một hoặc nhiều target cho iSCSI target để tạo một hoặc nhiều lun.
- Ta có thể chỉ định một hoặc nhiều target cho iSCSI initiator.
- Công cụ này cũng hỗ trợ kiểm soát ACL và xác thực CHAP.
  - **ALC (Access Control List)**: danh sách điều khiển truy nhập là một hạn chế truy nhập bằng cách sử dụng nút IQN để xác thực quyền truy nhập cho Client.
  - Giao thức xác thực **CHAP (Challenge-handshake authentication protocol)** **CHAP**: là mô hình xác thực dựa trên username và password.