# Basic about ``uptimerobot``

**Khái niệm**
- UptimeRobot là một trang web miễn phí giúp ta theo dõi trạng thái hoạt động của trang web.
  - Kiểm tra Ping.
  - Kiểm tra Port.
  - Kiểm tra dịch vụ.
- Nó sẽ theo dõi các trang web của ta cứ sau 5 phút và thông báo cho ta nếu trang web của bị sập.

**Cách thức hoạt động**
- Dưới đây là các hành động từng bước của Uptime Robot để hiểu rõ hơn về nó:

  - Nó sẽ giám sát trang web của ta cứ sau 5 phút (hoặc nhiều hơn tùy thuộc vào cài đặt)
  - Nếu Reason của trang web chỉ hiển thị ``ok``thì có nghĩa là trang web vẫn tốt.
  - Nếu Reason của trang web là ``Connection Timeout``, thì trang web hiện không connect được.
  - Khi trang web ngừng hoạt động, UptimeRobot sẽ thực hiện thêm một số kiểm tra trong 30 giây tiếp theo, để đảm bảo trang web ngừng hoạt động.
  - Nếu trang web vẫn không hoạt động, nó sẽ gửi một cảnh báo về telegram hoặc gmail...(do ta cài đặt)

### 1. Cài đặt

**Bước 1**: Truy cập vào: https://uptimerobot.com/ và đăng ký tài khoản bằng tài khoản gmail.

**Bước 2**: Khi đăng nhập vào nó sẽ hiển thị như dưới đây:
![](https://i.imgur.com/QTSQbnX.png)

**Bước 3**: Ta kích vào ``+ Add New Monitor`` để thêm trang web ta muốn theo dõi.
![](https://i.imgur.com/uafKCXL.png)

**Bước 4**: Khi kích vào nó sẽ hiển thị như dưới. Ta kích vào phần ``Monitor Type``

![](https://i.imgur.com/T4IOhRk.png)

- Còn tùy thuộc vào ta muốn theo dõi theo kiểu nào.
  
    ![](https://i.imgur.com/uj7Fo6w.png)

  - Trong đó:
     - ``HTTPS``: Nó sẽ quét cứ 5 phút 1 lần tới ``https`` của trang web
     - ``Keyword``: Nó sẽ gửi thông báo về gmail, telegram... khi có 1 một từ khóa ta cài đặt từ trước xuất hiện trên trang web, hoặc ngược lại ( do ta setup ).
     - ``Ping``: Nó sẽ ping tới trang web của ta để xem địa chỉ ip của ta có ổn định không.
     - ``Port``: Nó sẽ quét port của trang web, để xem port được bật, tắt khi nào.
   - Ở đây ta chọn kiểu ``https``:

![](https://i.imgur.com/Ox3qUuN.png)

Khi vào ``+ Add New Monitor`` ta có 2 phần cần chú ý:

**Phần 1:**
- Monitor Type*: là kiểu theo dõi.
- Friendly Name*: Là tên hiển thị thay thế cho đường dẫn.
- URL (or IP)*: Là Ta điền địa chỉ IP hoặc đường dẫn trang web ta muốn theo dõi.
- Monitoring Interval*: Là thời gian giám sát trang web. Có nghĩa là cứ sau 5 phút nó sẽ quét trang web của ta 1 lần để xem có vấn đề gì xảy ra không ( ở đây ta để 5 minutes ).

**Phần 2:**
- Ở đây ta tích vào gmail của ta, để khi có vấn đề xảy ra nó sẽ báo về đúng địa chỉ gmail ấy ( ta có thể add thêm 1 số ứng dụng để nó có thể gửi cảnh báo về ở trong phần ``My Settings``)

  - Cuối cùng sau khi lựa chọn xong ta nhấn ``Create Monitor`` để tạo:

**Bước 5**: Sao khi tạo xong một màn hình như dưới sẽ hiển thị:

![](https://i.imgur.com/bHkQxW1.png)

Ở đây ta chia làm 3 phần:

Trong đó:
**Phần 1:**

-  ``UP MONITORS``: 1 có nghĩa là tương ứng với số sang web ta theo dõi, Nhưng với điều kiện là vẫn hoạt động bình thường. Khi ta add thêm 1 trang web nữa sẽ là 2.
    ![](https://i.imgur.com/sLtRqOW.png)

- ``DOWN MONITORS``: có nghĩa là Khi trang web của ta ngừng hoạt động, nó sẽ hiển thị màu đỏ tương ứng với số trang web bị down ở trong phần ``DOWN MONITORS``

    ![](https://i.imgur.com/kW0V3hq.png)

- ``PAUSED MONITORS``: Có nghĩa là khi ta tạm dừng theo dõi 1 trang web nó sẽ hiển thị lên đấy với màu đen.

    ![](https://i.imgur.com/VKNZerJ.png)
    
**Phần 2:**
- Phần ``Overall Uptime``: là tổng thời gian hoạt động của trang web.
-  ``Latest downtime``: Hiển thị thời gian từ lúc trang web không hoạt động cho đến nay

**Phần 3:**
- Trong phần này sẽ hiển thị trạng thái, thời gian về trang web ta theo dõi.

    ![](https://i.imgur.com/o4wvRuu.png)

Trong đó:
- ``Event``: Hiển thị trạng thái của trang web.
- ``Monitor``: Hiển thị tên trang thay thế của trang web mà ta đã tạo.
- ``Date-Time``: Hiển thị ngày giờ thay đổi trạng thái gần nhất của trang web.
- ``Reason``: Cũng hiển thị trạng thái của trang web nhưng dưới dạng ``lý do``.
- ``Duration``: Nói lên thời gian của lần thay đổi trạng thái gần nhất.

### 2. Chỉnh múi giờ.
![](https://i.imgur.com/6fdf86y.png)

- Để chỉnh múi giờ ta có thể vào phần ``My settings``


### 3. Nhận thông báo

- Cũng ở phần ``My settings`` ta có thể add thêm cảnh báo về các trang khác nhau.

![](https://i.imgur.com/8UHQDUQ.png)

- Khi uptime nhận thấy trang web có thay đổi trạng thái thì nó sẽ báo về gmail, telegram...(tùy theo ta chỉnh)

![](https://i.imgur.com/LSaoGCA.png)

![](https://i.imgur.com/aBfBwcz.png)

## END
