# Cấu hình Apache cơ bản

**Ta có mô hình:**

![](https://i.imgur.com/oCLCxTz.png)

![](https://i.imgur.com/lhbfAmz.png)

**Phần 1: Triển khai:**

- Có 2 cách để cài đặt Apache

**Cách 1: cài đặt bằng lệnh apt-get**

B1: Ta update lại các gói trong hệ thống.
```
apt update
```

B2: Cài Apache.
```
apt install apache2
```

B3: Kiểm tra Firewall
```
ufw app list
```
Sau đó ta cho phép ``Apache`` hoạt động.
```
ufw allow 'Apache'
```
Kiểm tra lại.
```
ufw status
```

B4: Kiểm tra xem dịch vụ đang chạy chưa.
```
systemctl status apache2
```

![](https://i.imgur.com/9ejpsLy.png)

- Bây giờ ta có thể truy cập vào trang web apache

```
http://your_server_ip
```

- Ta sẽ thấy trang web Ubuntu 18.04 Apache mặc định:

![](https://i.imgur.com/SxP9ncM.png)

**Cách 2: Cài đặt Apache bằng source**

B1: Ta dùng lệnh ``wget`` để download file từ trang chủ của Apache về, lưu vào đâu ta muốn.

- Ở đây ta lưu vào thư mục ``/root``.
```
wget http://mirror.downloadvn.com/apache//httpd/httpd-2.4.39.tar.bz2
```

B2: Giải nén thư mục vừa download về.
```
tar jxvf httpd-2.4.39.tar.bz2
```

- Nó sẽ hiện ta thư mục mang tên ``httpd-2.4.39``

![](https://i.imgur.com/EFMiNSH.png)

B3: Biên dịch file:
```
cd httpd-2.4.39
./configure
make
make install
```

**Kiểm Tra phiên bản của apache:**
```
apachectl -v
```
![](https://i.imgur.com/pqyZ6n4.png)

**Phần 2: Tạo trang web của riêng mình**

- Theo mặc định, Apache đi kèm với một trang web cơ bản (trang web mà ta đã thấy trong bước trước) được bật. Ta có thể sửa đổi nội dung của nó trong /var/www/html hoặc cài đặt bằng cách chỉnh sửa tệp ``Virtual Host`` được tìm thấy trong /etc/apache2/sites-enabled/000-default.conf.
-  Ta sẽ để cấu hình  ``Virtual Host`` Apache mặc định trỏ đến www.example.com và thiết lập máy chủ tại ``example.com`` .

B1: Tạo thư mục cho ``example.com``
```
mkdir -p /var/www/example.com/html
```

B2: Gán quyền sở hữu thư mục. 
```
chown -R www-data:www-data /var/www/example.com/html
```

B3: Chỉnh quyền truy cập
```
chmod -R 755 /var/www/example.com
```

B4: Tạo một ``index.html`` để viết nội dung hiển thị
```
vi /var/www/example.com/html/index.html
```

- Bên trong index.html ta có thể viết:
```
 <html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

![](https://i.imgur.com/i4mJxRa.png)

Lưu tệp khi hoàn thành.

B5: Tạo một tệp Virtual Host để hiển thị những gì ta vừa tạo ở trên:

```
vi /etc/apache2/sites-available/example.com.conf
```
Trong file ``example.com.conf`` ta có thể viết như dưới:
```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    <Directory /var/www/example.com/html>
    AllowOverride AuthConfig FileInfo Limit Options=IncludesNOEXEC,MultiViews,SymLinksIfOwnerMatch,FollowSymLinks,None
    Options -ExecCGI -Includes +IncludesNOEXEC -Indexes
</Directory>
</VirtualHost>
```

![](https://i.imgur.com/R3GtS3t.png)

Lưu tệp khi hoàn thành.

- Folder ``site-Available``: Thư mục này có các tệp cấu hình cho Máy chủ ảo Apache2. Máy chủ ảo cho phép Apache2 được cấu hình cho nhiều trang web có cấu hình riêng biệt.
- Folder ``sites-enabled``: Chứa các liên kết đến thư mục /etc/apache2/sites-Available. Tương tự như vậy khi một tệp cấu hình trong ``sites-available được liên kết với nhau, trang được cấu hình bởi nó sẽ hoạt động sau khi Apache2 được khởi động lại.
  - Ta không nên chỉnh sửa trong thư mục ``sites-enable``

B6: Kích hoạt tệp với ``a2ensite``:
```
a2ensite example.com.conf
```

- a2 viết tắt của apache2 : en viết tắt của enable

B7: Vô hiệu hóa trang web mặc định được xác định trong 000-default.conf:
```
a2dissite 000-default.conf
```

- a2 viết tắt của apache2 : dis viết tắt của disable

B8: Kiểm tra lỗi cấu hình:
```
apache2ctl configtest
```
Ta sẽ thấy nó hiện như sau:

![](https://i.imgur.com/6lw7wI6.png)

B10: Reload để thực hiện các thay đổi của bạn:
```
service apache2 reload
```

- Bây là ta truy cập vào địa chỉ IP của Server Web
```
http://192.168.230.141
```

![](https://i.imgur.com/KNkx74X.png)

- Như vậy là ta đã setup Apache với Trang web của riêng mình.

**Phần 3: Cấu hình với tên miền**

- Vì ta không có tên miền thật nên ta sẽ phải điền tên miền ta vừa tạo trong Virtual Host vào trong file ``host``
- Đường dẫn: ``C:\Windows\System32\drivers\etc``
- Truy cập vào file ``host`` ta điền:

![](https://i.imgur.com/nAYteUk.png)

- Bây giờ ta có thể truy cập vào Server Web bằng tên miền ``http://www.example.com/``

![](https://i.imgur.com/i6AazPs.png)

**Phần 4: Cấu hình SSL**

B1: kích hoạt ssl.
```
a2enmod ssl
```

B2: kích hoạt file default-ssl

- Kích hoạt tệp cấu hình HTTPS mặc định trong ``/etc/apache2/sites-available/default-ssl.conf`` . Để Apache2 cung cấp HTTPS
```
a2ensite default-ssl
```

B3: restart lại dịch vụ
```
systemctl restart apache2.service
```

![](https://i.imgur.com/tlOgK04.png)

## END

