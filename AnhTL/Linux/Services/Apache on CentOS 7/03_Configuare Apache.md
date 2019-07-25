# Cấu hình Apache cơ bản

**Ta có mô hình:**

![](https://i.imgur.com/oCLCxTz.png)

![](https://i.imgur.com/sLnip5A.png)

**Triển khai:**

Phần 1: Cài đặt apache bằng lệnh yum

- **B1 :** Cài đặt repo `Epel` :
    ```
    # yum install -y epel-release
    ```
- **B2 :** Cài đặt gói `httpd` :
    ```
    # yum install -y httpd
    ```
khi cài qua lệnh ``yum`` thì , trên ``CentOS 6`` sẽ cài ``Apache 2.2`` , còn trên ``CentOS 7`` sẽ cài ``2.4`` .

**Phần 2:** Cài đặt Apache bằng source

- **B1 :** Download file về từ Internet và lưu vào thư mục `/var/tmp`
    ```
    # cd /var/tmp
    # wget https://archive.apache.org/dist/httpd/httpd-2.4.35.tar.gz
    ```
    > Có thể thay `2.4.35` bằng số khác để tải về các phiên bản khác
- **B2 :** Giải nén file vừa tải về :
    ```
    # tar -zxvf httpd-2.4.35.tar.gz
    ```
- **B3 :** Biên dịch file cài đặt :
    ```
    # cd httpd-2.4.35/
    # ./configure
    # make
    # make install
    ```

Để kiểm tra phiên bản ta dùng lệnh:
```
httpd -v
```

![](https://i.imgur.com/YTrumkN.png)

Như Vậy là ta đã có thể truy cập vào Server Web: ``http://192.168.230.140``

![](https://i.imgur.com/yIZCvmk.png)

**Ta có mô hình:**

![](https://i.imgur.com/oCLCxTz.png)

![](https://i.imgur.com/sLnip5A.png)

**B1:** Chỉnh sửa nội dung trang web ( file index.html ).
```
vi /var/www/html/index.html
```

<h1> Công Ty TNHH Phần Mềm Nhân Hòa </h1>

Nội dung viết tùy ý bạn.

![](https://i.imgur.com/K8OXbre.png)

**B2:** Khởi động dịch vụ httpd.
```
systemctl start httpd

systemctl enable httpd
```

**B3:** Cấu hình Firewalld cho phép dịch vụ http ( để các máy Client có thể truy cập ).
```
firewall-cmd --zone=public --permanent --add-service=http

firewall-cmd --reload
```

**B4:** Trên máy Client Windows 10: Ta vào trình duyệt web và gõ: ``http://192.168.230.140/``

![](https://i.imgur.com/nGWxzRB.png)


**disable Start page on Apache**

- Khi chưa tạo file ``index.html`` thì truy cập địa chỉ của Web_Server, nó sẽ mặc định hiện start page ra như dưới:

![](https://i.imgur.com/yIZCvmk.png)

- Nếu không muốn hiện Start page, ta có thể xóa file ``welcome.conf`` để đến thẳng folder Web:

```
rm -f /etc/httpd/conf.d/welcome.conf

systemctl restart httpd
```

![](https://i.imgur.com/mqNBCdG.png)
