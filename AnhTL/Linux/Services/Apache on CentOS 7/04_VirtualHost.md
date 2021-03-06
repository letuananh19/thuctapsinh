# Virtual host


### 1. Khái niệm

**Virtual Host** là một cấu hình trong Apache để cho phép nhiều domain cùng chạy trên một máy chủ. 
- Có 2 dạng virtual host:
  - **Port-based**: Mỗi Website gán vào 1 port của IP
  - **Name-based**: Nhiều tên miền chạy trên 1 địa chỉ IP

### 2. Cấu hình
**2.1: IP-based**

- **B1:** Thêm các port khác nhau cho các IP**
  - Chỉnh sửa file ``httpd.conf``
  ```
  vi /etc/httpd/conf/httpd.conf
  :set nu (để hiển thị số dòng)
  ```
  
    - Tìm tới dòng 42 và thêm các port mở thêm ( số lượng port LISTEN bằng với số website ):
  ![](https://i.imgur.com/YqoflKH.png)
  
  - Khởi động lại dịch vụ httpd :
  ```
  systemctl restart httpd
  ```

- **B2:** Thêm dữ liệu cho các trang web :

  - Tạo thư mục riêng để lưu chữ cho file VirtualHost:

```
mkdir /etc/httpd/sites-available
mkdir /etc/httpd/sites-enabled
```

  - Để apache server chạy các file từ directory sites-enabled , thêm lệnh sau vào cuối file httpd.conf :

```
vi /etc/httpd/conf/httpd.conf

IncludeOptional sites-enabled/*.conf
```

- **B3:** Cấu hình Virtual host:
  - Tạo các file có đuôi ``.conf`` cho từng virtual host:
  - Phần <VirtualHost *:port> phải nhập đúng với số port Listen
```
# vi /etc/httpd/sites-available/web1.conf
<VirtualHost *:80>
    ServerName web1.com
    ServerAlias www.web1.com
    DocumentRoot /var/www/html/web1
    ErrorLog /var/www/html/web1/error.log
    CustomLog /var/www/html/web1/access.log combined
</VirtualHost>
```
```
# vi /etc/httpd/sites-available/web2.conf
<VirtualHost *:8080>
    ServerName web2.com
    ServerAlias www.web2.com
    DocumentRoot /var/www/html/web2
    ErrorLog /var/www/html/web2/error.log
    CustomLog /var/www/html/web2/access.log combined
</VirtualHost>
```
```
# vi /etc/httpd/sites-available/web3.conf
<VirtualHost *:9000>
    ServerName web3.com
    ServerAlias www.web3.com
    DocumentRoot /var/www/html/web3
    ErrorLog /var/www/html/web3/error.log
    CustomLog /var/www/html/web3/access.log combined
</VirtualHost>
```
  
  - Trong đó :
  
``<VirtualHost></VirtualHost>`` Đây là cặp thẻ báo hiệu mở đầu và kết thúc của một khai báo về Vhost. 

``ServerName``: tên của website dùng để gõ trên trình duyệt

``ServerAlias``: tên gọi khác của website ( thường cấu hình www hoặc non www)

``DocumentRoot``: đường dẫn đến thư mục source code

``ErrorLog``: thư mục chứa file log lỗi

``CustomLog``: thư mục chứa file log truy cập

- **B4:** Enable Virtual Host Files :

  - Ta tạo các symbolic link của các file cấu hình virtual host vào trong directory sites-enabled:
```
sudo ln -s /etc/httpd/sites-available/web1.conf /etc/httpd/sites-enabled/web1.conf
sudo ln -s /etc/httpd/sites-available/web2.conf /etc/httpd/sites-enabled/web2.conf
sudo ln -s /etc/httpd/sites-available/web3.conf /etc/httpd/sites-enabled/web3.conf
```

  - Sau đó ta restart lại service để áp dụng các thay đổi :
```
sudo apachectl restart 
```

- **B5:** Cấu hình bên client :

  - Do không có một domain thật , nên ta cần cấu hình file hosts ở một máy client để có thể truy cập vào web server (Ở ví dụ đây máy client chạy win 10 ):
```
vi /etc/hosts
```
192.168.230.140 www.web1.com

192.168.230.140 www.web2.com

192.168.230.140 www.web3.com
```

# END
