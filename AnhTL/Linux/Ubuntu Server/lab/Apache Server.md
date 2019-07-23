# Setup Apache Server on Ubuntu

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