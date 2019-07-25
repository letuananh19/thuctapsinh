# Apache

## Mục lục:

[1. Khái niệm](#1)

[2. Cách thức hoạt động](#2)

[3. Ưu điểm và hạn chế của Apache](#3)

- [3.1: Ưu điểm](#3.1)
- [3.2: Nhược điểm](#3.2)

[4. Các file/thư mục quan trọng của Apache](#4)

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

### <a name="4"> 4. Các file/thư mục quan trọng của Apache </a>

**Nội dung:**

- **/var/www/html**: Nội dung mặc định được hiển thị trên trang web được đặt ở đây. Điều này có thể được thay đổi bằng cách thay đổi các tệp cấu hình Apache.

**Cấu hình máy chủ**

- **/etc/apache2**: Thư mục cấu hình Apache. Tất cả các tệp cấu hình Apache nằm ở đây.
- **/etc/apache2/apache2.conf**: Tệp cấu hình chính của Apache. Điều này có thể được sửa đổi. Tệp này chịu trách nhiệm tải nhiều tệp khác trong thư mục cấu hình.
- **/etc/apache2/ports.conf**: Tệp này chỉ định các cổng mà Apache sẽ nghe. Theo mặc định, Apache lắng nghe trên cổng 80 và nghe thêm trên cổng 443 khi mô-đun cung cấp khả năng SSL được bật.
- **/etc/apache2/sites-available/**: Thư mục này là nơi lưu trữ máy chủ ảo trên mỗi trang web có thể được lưu trữ. Apache sẽ không sử dụng các tệp cấu hình được tìm thấy trong thư mục này trừ khi chúng được liên kết với thư mục **sites-enabled**. Thông thường, tất cả cấu hình khối máy chủ được thực hiện trong thư mục này, và sau đó được kích hoạt bằng cách liên kết với thư mục khác bằng lệnh ``a2ensite``.
- **/etc/apache2/sites-enabled/**: Thư mục này là nơi kích hoạt máy chủ ảo trên mỗi trang web được lưu trữ. Thông thường, chúng được tạo bằng cách liên kết đến các tệp cấu hình được tìm thấy trong thư mục **sites-available** với lệnh ``a2ensite``. Apache đọc các tệp cấu hình và các liên kết được tìm thấy trong thư mục này khi nó khởi động hoặc tải lại để biên dịch một cấu hình hoàn chỉnh.
- **/etc/apache2/conf-available/** và **/etc/apache2/conf-enabled/**: Các thư mục này có cùng mối quan hệ với các thư mục **sites-available** và **sites-enabled**, nhưng được sử dụng để lưu trữ các đoạn cấu hình không thuộc về máy chủ ảo. Các tập tin trong thư mục **conf-available** có thể được kích hoạt bằng lệnh ``a2enconf`` và vô hiệu hóa bằng lệnh ``a2disconf``.
- **/etc/apache2/mods-available/** và **/etc/apache2/mods-enabled/**: Các thư mục này chứa các mô-đun có sẵn và được kích hoạt. Các tệp kết thúc bằng .load chứa các đoạn để tải các mô-đun cụ thể. conf chứa cấu hình cho các mô-đun đó. Các mô-đun có thể được kích hoạt và vô hiệu hóa bằng cách sử dụng lệnh ``a2enmod`` và ``a2dismod``.

**Nhật ký máy chủ**

- **/var/log/apache2/access.log**: Theo mặc định, mọi yêu cầu đến máy chủ web của bạn được ghi lại trong tệp nhật ký này trừ khi Apache được cấu hình để làm khác.
- **/var/log/apache2/error.log**: Theo mặc định, tất cả các lỗi được ghi lại trong tập tin này. Lệnh ``LogLevel`` trong cấu hình Apache chỉ định mức độ chi tiết của các bản ghi lỗi.


