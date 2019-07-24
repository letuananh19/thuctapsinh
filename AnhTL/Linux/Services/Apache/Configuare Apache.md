# Cấu hình Apache cơ bản

**Ta có mô hình:**

![](https://i.imgur.com/oCLCxTz.png)

![](https://i.imgur.com/sLnip5A.png)

**Triển khai:**

**B1:** Chỉnh sửa file httpd.conf
```
vi /etc/httpd/conf/httpd.conf
```

Sau khi vào ta dùng lệnh `` :set nu `` để hiển thị số dòng

![](https://i.imgur.com/ZWaTGox.png)

![](https://i.imgur.com/lBmziHI.png)

- Ta chỉnh sửa 1 số thứ:
  - Ở dòng 86, sửa ``localhost``thành tên domain của ta:
  ![](https://i.imgur.com/JhuQY65.png)
  - Ở dòng 95, sửa ``www.example.com`` thành tên domain , đồng thời bỏ dấu ``#`` ở đầu dòng:
  ![](https://i.imgur.com/p3QSWUO.png)
  - Ở dòng 119, chỉ định đường dẫn tới thư mục chính lưu nội dung trang web:
  ![](https://i.imgur.com/PQHrsZv.png)
  - Ở dòng 151, sửa none thành All:
  ![](https://i.imgur.com/SLpQTMW.png)
  - Ở dòng 164, chỉ định file index.html là nội dung chính của website :
  ![](https://i.imgur.com/e7BcTiD.png)

Xong thì ta lưu và thoát.

**B2:** Chỉnh sửa nội dung trang web ( file index.html ).
```
vi /var/www/html/index.html
```

<h1> Công Ty TNHH Phần Mềm Nhân Hòa </h1>

Nội dung viết tùy ý bạn.

![](https://i.imgur.com/K8OXbre.png)

**B3:** Khởi động dịch vụ httpd.
```
systemctl start httpd

systemctl enable httpd
```

**B4:** Cấu hình Firewalld cho phép dịch vụ http ( để các máy Client có thể truy cập ).
```
firewall-cmd --zone=public --permanent --add-service=http

firewall-cmd --reload
```

**B5:** Trên máy Client Windows 10: Ta vào trình duyệt web và gõ: ``http://192.168.230.140/``

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