# Setup Apache Server on Ubuntu

![](https://s24255.pcdn.co/wp-content/uploads/2014/04/apache.jpg)

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

![](https://i.imgur.com/ta34WZF.png)

**Phần 2: Thiết lập máy chủ ảo**

Khi sử dụng máy chủ web Apache, ta có thể sử dụng máy chủ ảo để đóng gói chi tiết cấu hình và lưu trữ nhiều hơn một tên miền từ một máy chủ. Ta sẽ thiết lập một tên miền được gọi là ``example.com`` ( Có thể thay thế bằng tên miền khác ) . Với điều kiện là tên miền đã được thuê.

B1: Tạo thư mục cho ``example.com``
```
mkdir -p /var/www/example.com/html
```

B2: Gán quyền sở hữu thư mục. 
```
chown -R $USER:$USER /var/www/example.com/html
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

B5: Tạo một tệp máy chủ ảo mới tại: ``/etc/apache2/sites-available/example.com.conf``
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

B10: Khởi động lại Apache để thực hiện các thay đổi của bạn:
```
systemctl restart apache2
```

- Như vậy là ta đã setup Apache với tên miền. Ta có thể kiểm tra điều này bằng cách nhập: http://example.com

- Ta có thể dùng lệnh dưới đây để show ra cấu hình file ``virtualhost``:
```
apache2ctl -S
```

## END
