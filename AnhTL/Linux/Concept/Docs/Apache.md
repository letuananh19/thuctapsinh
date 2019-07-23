# Apache

## Mục lục:

[1. Khái niệm](#1)

[2. Cách thức hoạt động](#2)

[3. Ưu điểm và hạn chế của Apache](#3)

- [3.1: Ưu điểm](#3.1)
- [3.2: Nhược điểm](#3.2)

[4. Setup Apache](#4)

--------------------

![](https://s24255.pcdn.co/wp-content/uploads/2015/03/Apache-http-server.png)

### <a name="1"> 1. Khái niệm </a>
Apache là phần mềm web server miễn phí mã nguồn mở. Tên chính thức của Apache là Apache HTTP Server, được điều hành và phát triển bởi Apache Software Foundation.

Apache hoạt động trên port 80

Các yêu cầu gửi tới máy chủ sử dụng phương thức HTTP còn được gọi tắt là HTTP request.

- **Ví dụ**: khi ta sử dụng trình duyệt ta có thể gửi đi một HTTP request đơn giản bằng cách nhập một địa chỉ IP (hoặc một URL chứa tên miền). Khi đó là ta đã gửi đi một HTTP request tới một máy chủ trên internet. Địa chỉ máy chủ này được xác định bởi địa chỉ IP (hoặc URL với tên miền) mà ta đã nhập vào.

Do được cài đặt trên web server (phần cứng) nên Apache được gọi là web server hay HTTP server. 

Apache có các phiên bản dưới đây:
![](https://i.imgur.com/RRlQ9YK.png)

### <a name="2"> 2. Cách thức hoạt động </a>
Tuy được gọi là ``Apache web server`` nhưng nó không phải là server vật lý mà **Apache** chính là một phần mềm chạy trên server đó. Nhiệm vụ chính của Apache là thiết lập kết nối giữa server và browser (Firefox, Chrome, Safari,...), sau đó chịu trách nhiệm chuyển file qua lại giữa giữa server và browser (cấu trúc hai chiều client-server). Apache hoạt động tốt với cả server Unix và Windows và là phần mềm đa nền tảng.

- **Ví dụ**: Khi user tải một site trên trang web, browser của user sẽ gửi request tải trang đó lên server và Apache sẽ trả lại kết quả với đầy đủ toàn bộ các file, các thành phần để hiển thị hoàn chỉnh trang web (bao gồm image, text,...). Server và client giao tiếp với nhau qua HTTP protocol và Apache chịu trách nhiệm đảm bảo quá trình này diễn ra trơn tru và bảo mật giữa hai máy.

Apache là một nền tảng module có độ tùy biến khá cao. Modules cho phép quản trị viên server có thể tắt hoặc thêm vào các chức năng. Apache sở hữu các modules cho bảo mật caching, URL rewriting, chứng thực mật khẩu,...

### <a name="3"> 3. Ưu điểm và hạn chế của Apache </a>
Apache web server là lựa chọn khá tối ưu nếu ta muốn vận hành website của mình một cách thật ổn định và có thể tùy chỉnh linh hoạt.

**<a name="3.1"> 3.1: Ưu điểm </a>**
- Apache là phần mềm mã nguồn mở, miễn phí kể cả cho mục đích thương mại.

- Apache đáng tin cậy, ổn định.

- Apache luôn được cập nhật thường xuyên, được vá lỗi bảo mật liên tục.

- Apache khá linh hoạt vì có cấu trúc module.

- Apache dễ dàng cấu hình, thân thiện với người mới bắt đầu sử dụng.

- Apache là phần mềm đa nền tảng (Unix và Windows).

- Apache hoạt động cực kỳ hiệu quả với các website WordPress.

- Apache sở hữu một cộng đồng lớn và sẵn sàng hỗ trợ bất kỳ lúc nào khi ta gặp sự cố.

**<a name="3.2"> 3.2: Nhược điểm </a>**
- Gặp vấn đề hiệu năng nếu website có lượng traffic cực lớn.

- Quá nhiều tùy chọn trong thiết lập gây ra các điểm yếu về bảo mật.

### <a name="4"> 4. Setup Apache </a>

https://github.com/letuananh19/thuctapsinh/blob/master/AnhTL/Linux/Ubuntu%20Server/lab/Apache%20Server.md

## END

## Tài liệu tham khảo

https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04-quickstart

https://tech.vccloud.vn/apache-la-gi-cach-cai-dat-apache-20181015091757724.htm

https://sanvu88.net/cai-dat-apache-tren-ubuntu.html
