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
