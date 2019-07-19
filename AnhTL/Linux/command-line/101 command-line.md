# 101 command-line

1: Hiển thị thông tin hệ thống:
```
dmidecode
```

1.1: Hiển thị thông tin bios:
```
dmidecode -t 0
```

1.2: Hiển thị thông tin cpu:
```
dmidecode -t 4
```

- Ta có thể dùng lệnh ``man dmidecode`` để hiện thị thêm các options của lệnh dmidecode.

2: Kiểm tra xem máy chủ là ảo hay vậy lý:
```
systemd-detect-virt
```

3: Hiển thị ngày,giờ,tháng,năm của máy tính:
```
date
```

4: Hiển thị tất cả các gói đã cài đặt:
```
dpkg --list
```

5: Ước tính không gian đĩa, tệp:
```
du -bh (đường dẫn)
```

![](https://i.imgur.com/kl1KLxG.png)

6: Hiển thị memory đã sử dụng:
```
free
```
```
-b : hiển thị dưới dạng Byte (B)
-k : hiển thị dưới dạng Kilo Bytes (KB)
-m : hiển thị dưới dạng Megabytes (MB)
-g : hiển thị dưới dạng Gigabytes (GB)
-t : hiển thị tổng cộng số RAM
```

7: Hiển thị thông tin về hệ thống hiện tại:
```
uname 
```

7.1: Hiển thị tên máy chủ:
```
uname -n
```

```
-a : hiển thị toàn bộ thông tin.
-s : hiển thị tên kernel.
-n : hiển thị tên máy chủ.
-r : hiển thị phiên bản hiện tại của kernel.
-p : hiển thị loại bộ xử lý.
-o : hiển thị hệ điều hành .
```

8: Tìm kiếm một chuỗi trong một file:
```
grep [OPTIONS] chuỗi [FILE...]
```

8.1: Hiển thị tất cả các từ " swap " trong thư mục /etc
```
grep -r swap /etc
```

- Dùng lệnh `` man grep `` để hiện thị thêm các option

9: Hiển thị user id (uid) và group id (gid) của user hiện đang sử dụng:
```
id
```

10: Hiển thị địa chỉ IP và netmask trên máy:
```
ifconfig
```
```
ip a
```

11: Hiển thị khoảng thời gian shutdown gần đây nhất:
```
last -x | grep shutdown | head -1
```

12: Hiển thị các file, thư mục không bị ẩn trong thư mục hiện tại. Có thể sử dụng option``-a`` để hiển thị cả những file bị ẩn:
```
ls
```

12.1: Hiển thị quyền truy cập đối với tất cả files bên trong thư mục:
```
ls -l filename
```

13: Hiển thị tất cả các lệnh có sẵn:
```
ls /usr/bin
```

14: Hiển thị các modules hiện đang load trong kernel:
```
lsmod
```

15: Show bảng định tuyến
```
netstat -rn
```

16: Hiển thị địa chỉ gateway mặc định
```
route
```

17: Xóa hoàn toàn mọi thông tin và dấu vết của 1 file không cho khôi phục lại. 
```
shred -zuv -n 7 <file>
```

Trong đó: 
- -n 7 ghi đè lên file 7 lần. 
- -z ghi đè lần cuối bằng không để ẩn shred.
- -u thực hiện xóa file khi thự hiện xong.
- -v hiển thị quá trình thực hiện.

18: Shutdown now:
```
shutdown -h now
```

19: Restart now:
```
shutdown -r now
```

20: SSH đến máy tính khác:
```
ssh <username>@<IP>
```

21: In ra tên của terminal ta đang sử dụng:
```
tty
```

22: Trả về vị trí của một chương trình trong hệ thống:
```
whereis <command>
```

23: Tră về đường dẫn của một ứng dụng:
```
which <command>
```

24: In ra những người đang truy cập vào máy:
```
who
```

25: In ra tên mà ta đang đăng nhập vào máy:
```
whoami
```

26: Tìm file
```
find <đường_dẫn> -name <filename>
```
27: Hiện thị trạng thái của file
```
stat filename.txt
```

28: In ra danh sách 10 người đăng nhập cuối cùng vào máy
```
last -n 10
```

29: Hiển thị tiến trình của hệ thống dưới dạng tree
pstree















