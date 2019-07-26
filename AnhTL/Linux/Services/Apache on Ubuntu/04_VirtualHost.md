## Cấu hình VirtualHost

- Theo mặc định, Apache đi kèm với một trang web cơ bản (trang web mà ta đã thấy trong bước trước) được bật. Ta có thể sửa đổi nội dung của nó trong /var/www/html hoặc cài đặt bằng cách chỉnh sửa tệp ``Virtual Host`` được tìm thấy trong /etc/apache2/sites-enabled/000-default.conf.
-  Ta sẽ để cấu hình  ``Virtual Host`` Apache mặc định trỏ đến www.example.com và thiết lập máy chủ tại ``example.com`` .

**Phần 1: Tạo trang web cho riêng mình:**

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

![](https://i.imgur.com/P1Tcti7.png)

Tuy nhiên đây là lỗi phổ biến trong Ubuntu 18.04:

Để khắc phục ta dùng 3 lệnh này để khắc phục:
```
echo "ServerName example.com" | sudo tee /etc/apache2/conf-available/servername.conf
```
```
a2enconf servername
```
```
systemctl reload apache2
```

Bây giờ ta dùng lại lệnh để kiểm tra:
```
apache2ctl configtest
```

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

- Như vậy là ta đã setup Apache thành công của riêng mình.

**Phần 2: Cấu hình với tên miền:**
- ``Nếu có tên miền thì bỏ qua phần này!``

- Vì ta không có tên miền thật nên ta sẽ phải điền tên miền ta vừa tạo trong Virtual Host vào trong file ``host``
- Đường dẫn: ``C:\Windows\System32\drivers\etc``

- Truy cập vào file ``host`` ta điền:

![](https://i.imgur.com/nAYteUk.png)

- Bây giờ ta có thể truy cập vào Server Web bằng tên miền ``http://www.example.com/``

![](https://i.imgur.com/i6AazPs.png)

**Phần 3: Một số lệnh quản lý Apache phổ biến**

**3.1**: Sử dụng lệnh này là sudo để khởi động máy chủ Apache.

```
sudo systemctl start apache2
```

Sử dụng lệnh này để dừng máy chủ Apache khi nó ở chế độ khởi động.

```
sudo systemctl stop apache2
```

Sử dụng lệnh này để dừng và sau đó khởi động lại dịch vụ Apache.

```
sudo systemctl restart apache2
```

Sử dụng lệnh này để áp dụng các thay đổi cấu hình mà không cần khởi động lại kết nối.

```
sudo systemctl reload apache2
```

Sử dụng lệnh này để cho phép Apache được khởi động mỗi khi ta khởi động hệ thống của mình.

```
sudo systemctl enable apache2
```

Sử dụng lệnh này để tắt nếu ta đã thiết lập Apache để được khởi động mỗi khi bạn khởi động hệ thống của mình.

```
sudo systemctl disable apache2
```
