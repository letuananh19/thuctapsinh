# Cấu hình Apache cơ bản

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

**Phần 2: Kiểm Tra phiên bản của apache:**
```
apachectl -v
```
![](https://i.imgur.com/pqyZ6n4.png)

**Phần 3: Một số lệnh quản lý Apache phổ biến**

**3.1**: Sử dụng lệnh này là sudo để khởi động máy chủ Apache.

```
sudo systemctl start apache2
```

**3.2**: Sử dụng lệnh này để dừng máy chủ Apache khi nó ở chế độ khởi động.

```
sudo systemctl stop apache2
```

**3.3**: Sử dụng lệnh này để dừng và sau đó khởi động lại dịch vụ Apache.

```
sudo systemctl restart apache2
```

**3.4**: Sử dụng lệnh này để áp dụng các thay đổi cấu hình mà không cần khởi động lại kết nối.

```
sudo systemctl reload apache2
```

**3.5**: Sử dụng lệnh này để cho phép Apache được khởi động mỗi khi ta khởi động hệ thống của mình.

```
sudo systemctl enable apache2
```

**3.6**: Sử dụng lệnh này để tắt Apache.

```
sudo systemctl disable apache2
```
