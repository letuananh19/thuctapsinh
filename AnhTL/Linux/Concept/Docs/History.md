# History in linux

**Note: Hãy cẩn thận khi sử dụng các lệnh history vì nó có thể làm dối loạn hệ thống của bạn**

**1 Số cú pháp về lệnh: ``history``**

1. In ra lịch sử:
```
history
```

2. In ra các dòng lịch sử gần nhất:
```
history n
```
Trong đó: n là số dòng ta muốn hiện.

![](https://i.imgur.com/HwOqbzj.png)

3. Lặp lại lệnh gần nhất:
```
!!
```

![](https://i.imgur.com/WZOjiIi.png)

- Hoặc ta có thể sử dụng mũi tên lên để hiển thị lại.

4. Lặp lại lệnh cụ thể:
```
! số_dòng_của_lệnh
```

![](https://i.imgur.com/jT7RzBo.png)

5. Lặp lại lệnh bắt đầu với một chuỗi ký tự:
```
root@ubuntu-server:~# systemctl start apache2
root@ubuntu-server:~# systemctl stop chronyd
root@ubuntu-server:~# systemctl restart chronyd
root@ubuntu-server:~# !systemctl
systemctl restart chronyd
```

![](https://i.imgur.com/wOUyAAl.png)

- Như hình trên, lệnh gần đây nhất bắt đầu bằng 'systemctl' đã được chạy lại.
- Mặc dù hữu ích, điều này có thể nguy hiểm nếu lệnh cuối cùng của ta thực sự khác với những gì ta đã thực hiện. Ta chạy lệnh này với ``: p`` ở cuối để in lệnh thay vì thực hiện lại lệnh ngay lập tức.

![](https://i.imgur.com/Cr1rRxL.png)
- Nó chỉ hiển thị lệnh chứ không thực thi lệnh.

6. Tìm kiếm trong history:
```
history | grep apache2
```

![](https://i.imgur.com/LRFFZew.png)

- Với grep là lệnh tìm kiếm chuỗi

7. Ghi vào file history
```
history -w
```

- Ta có thể ghi history vào file ``/root/.bash_history`` để dù kẻ xấu có xóa lịch sử thì ta vẫn có thể kiểm tra lịch sử trong file ``.bash_history``.
- Đường dẫn của file: ``/root/.bash_history``

8. Xóa toàn bộ history:
```
history -c
```

- Lưu ý rằng điều này sẽ chỉ xóa lịch sử trong bộ nhớ, những thay đổi sẽ được ghi khi người dùng đăng xuất tuy nhiên chúng ta có thể lưu các thay đổi vào tệp ``.bash_history`` ngay lập tức bằng cách chạy ``history -w``.
- Tệp ``.bash_history`` vẫn có thể xóa được.

9. Xóa dòng history cụ thể:
```
history -d dòng_muốn_xóa
```

![](https://i.imgur.com/xfN3OXg.png)

10. Chạy lệnh mà không bị ghi vào history:
```
echo "command-line";history -d $(history 1)
```

11. Tăng kích thước lưu history:

Theo mặc định, history sẽ lưu trữ được 1000 dòng, các giá trị được lưu trữ trong các biến ``$HISTSIZE`` và ``$HISTFILESIZE``.

```
root@ubuntu-server:~# echo $HISTSIZE
1000
root@ubuntu-server:~# echo $HISTFILESIZE
1000
```

Mặc định cho tất cả user được lưu trữ trong tệp /etc/profile, điều này có thể được sửa đổi hoặc ta có thể nối các dòng sau vào dưới cùng của ~/.bashrc sẽ áp dụng cho người dùng đó vào lần đăng nhập tiếp theo.

```
HISTSIZE=2000
HISTFILESIZE=2000
```

![](https://i.imgur.com/yXdALZF.png)

![](https://i.imgur.com/P4tufMW.png)

12. Thêm dấu thời gian vào history:
```
echo 'export HISTTIMEFORMAT="%c "' >> ~/.bashrc
```

Sau đó reboot rồi dùng lệnh history để kiểm tra thử.

![](https://i.imgur.com/uRvQe3U.png)

13. Thay đổi vị trí tệp ``~/.bash_history``

Theo mặc định, lịch sử bash được ghi vào ~/.bash_history,

```
echo 'export HISTFILE="/root/new_history"' >> ~/.bashrc
```

Ta reboot. Sau đó tất cả lịch sử sẽ được lưu trữ trong ``/root/new_history``.

14. Không lưu trữ các lệnh trùng lặp:

Theo mặc định /etc/profile đặt biến $HISTCONTROL thành 'ignoredups', sẽ bỏ qua các lệnh trùng lặp được chạy lần lượt.

![](https://i.imgur.com/pUOCHCA.png)

![](https://i.imgur.com/2Qvw6ZD.png)

15. Tìm kiếm ngược:

Nhấn phím: ctrl + r


Ta sẽ thấy dấu nhắc (Reverse-i-search) `': tại thời điểm này, ta có thể bắt đầu nhập một lệnh đã được thực thi trước đó và nó sẽ hiển thị lệnh gần đây nhất.

![](https://i.imgur.com/8qzkm3C.png)