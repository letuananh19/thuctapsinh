# Tạo trang web của riêng mình với VirtualHost

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

- Trong đó:

  - ServerName : tên của website dùng để gõ trên trình duyệt
  - ServerAlias : tên gọi khác của website ( thường cấu hình www hoặc non www)
  - DocumentRoot : đường dẫn đến thư mục source code
  - ErrorLog : thư mục chứa file log lỗi
  - CustomLog : thư mục chứa file log truy cập

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

- Như vậy là ta đã setup Apache với tên miền. Ta có thể kiểm tra điều này bằng cách nhập: http://example.com

**Phần 3: Cấu hình với tên miền**

- Vì ta không có tên miền thật nên ta sẽ phải điền tên miền ta vừa tạo trong Virtual Host vào trong file ``host``
- Đường dẫn: ``C:\Windows\System32\drivers\etc``

- Truy cập vào file ``host`` ta điền:

![](https://i.imgur.com/nAYteUk.png)

- Bây giờ ta có thể truy cập vào Server Web bằng tên miền ``http://www.example.com/``
