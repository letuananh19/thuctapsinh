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