# iSCSI - Internet Small Computer System Interface

![](https://community.fs.com/blog/wp-content/uploads/2018/08/how-iSCSI-storage-works.jpg)

### Mục lục
[1. Khái niệm](#1)

[2. Các thành phần của iSCSI](#2)

- [2.1 iSCSI Initiator](#2.1)
- [2.2 iSCSI Target](#2.2)

[3. Một số thuật ngữ trong iSCSI](#3)

[4. iSCSI hoạt động như thế nào?](#4)

[5. Một số loại bộ nhớ sao lưu (Backstore)](#5)

[6. Cài đặt](#6)

[Tài liệu tham khảo](#tltk)

[Thông tin liên quan]

https://github.com/letuananh19/thuctapsinh/blob/master/AnhTL/Linux/Concept/Docs/iSCSI%20Helper.md

------------------

#### <a name="1"> 1. Khái niệm </a>

- **iSCSI** được xem là một giao thức để truyền tải các khối dữ liệu qua mạng IP bằng giao thức TCP/IP. Từ đó iSCSI cho phép truy cập các khối dữ liệu (phân vùng, disk, hoặc thiết bị lưu trữ) trên server qua các lệnh SCSI và truyền tải dữ liệu trên hệ thống mạng(LAN/WAN).
- **iSCSI** sử dụng cổng TCP 3260.
#### <a name="2"> 2. Các thành phần của iSCSI </a>

Một kết nối iSCSI sẽ chia ra làm 2 thành phần chính.

- **iSCSI Initiator**
- **iSCSI Target**

![](https://cuongquach.com/resources/images/2018/01/initiator-target-iscsi.png)

**<a name="2.1"> 2.1 iSCSI Initiator </a>**

- Là máy client. Nó gửi yêu cầu truy cập đến server để truy cập vào khối dữ liệu và truyền tải các lệnh iSCSI qua đường truyền mạng TCP/IP.

**<a name="2.2"> 2.2 iSCSI Target </a>**

- Đóng vai trò server, nơi lưu trữ dữ liệu. Mỗi máy target phải có một tên duy nhất để các initiator truy cập vào. Một target có một hoặc nhiều hơn 1 khối các thiết bị lưu trữ.

- Từ máy chủ **iSCSI Target** này sẽ tiếp nhận các request từ **iSCSI Initiator** gửi đến và trả dữ liệu về 

- Trên **iSCSI Target** sẽ quản lý ổ đĩa iSCSI với các tên gọi **LUN (Logical Unit Number)** được dùng để chia sẻ ổ đĩa lưu trữ iSCSI với phía iSCSI Client.

#### <a name="3"> 3. Một số thuật ngữ trong iSCSI </a>

- **Initiator**: Là máy client. Nó gửi yêu cầu truy cập đến server để truy cập vào khối dữ liệu.
- **Target**: đóng vai trò server, nơi lưu trữ dữ liệu. Mỗi máy target phải có một tên duy nhất để các initiator truy cập vào. Một target có một hoặc nhiều hơn 1 khối các thiết bị lưu trữ.
- **ALC (Access Control List)**: danh sách điều khiển truy nhập là một hạn chế truy nhập bằng cách sử dụng nút IQN để xác thực quyền truy nhập cho Client.
- **Discovery**: Nó liên quan đến việc truy vấn vào server để tìm kiếm mục tiêu được cấu hình.
- **iqn**: Là một quy định trong cách đặt tên cho cả target và initiator. Định dạng đặt tên như sau: ```iqn.năm-tháng.tên_miền:tên_phân_biệt``` Trong đó:
  - ``iqn``: Biểu thị rằng tên này sẽ sử dụng tên miền làm mã định danh cho nó.
  - ``Năm-tháng``: là tháng đầu tiên mà tên miền được tạo.
  - ``Tên miền``: là tên của server.
  - ``Tên phân biệt``: Tên này được thêm vào để ta phân biệt với target khác trên máy này.
- **Login**: Điều này xác thực target hoặc LUN để bắt đầu sử dụng dữ liệu từ phía client.
- **LUN- Logical Unit Number**: Gồm các thiết bị (disk, partition hoặc logical volume) được gắn kết với nhau và thông qua target. Một target có thể gồm nhiều LUN nhưng thông thường một target cung cấp một LUN.
- **node**: Là một iSCSI initiator hoặc iSCSI target được xác định bởi IQN của nó.
- **portal**: Là địa chỉ IP và port trên target và initiator để thiết lập kết nối.

#### <a name="4"> 4. iSCSI hoạt động như thế nào? </a>

![](http://i1.wp.com/opentodo.files.wordpress.com/2012/10/2iscsiprotocol.jpg)

- **iSCSI Initiator** sẽ khởi tạo request yêu cấu truy xuất dữ liệu đến hệ thống lưu trữ store ở máy chủ **iSCSI Target**.
- Lúc này Initiator sẽ tạo ra 1 số lệnh **SCSI** tương ứng với yêu cầu của client.
- Các lệnh **SCSI** và thông tin liên quan sẽ được đóng gói trong **gói tin iSCSI Protocol Data Unit (iSCSI PDU)**
- Thông tin **PDU** được sử dụng cho kết nối giữa Initiator và Target với các thông tin nhằm xác định node, kết nối, thiết lập session, truyền tải lệnh SCSI và truyền tải dữ liệu.
- Sau đó **PDU** được đóng gói trong mô hình TCP/IP và truyền tải đến máy chủ iSCSI Target.

![](https://cuongquach.com/resources/images/2018/01/iscsi-tcp-ip.jpg)

- Máy chủ **iSCSI Target** nhận được và tiến hành mở gói tin ta, kiểm tra phần **iSCSI PDU** nhằm trích xuất thông tin lệnh **SCSI** và các nội dung liên quan.
- Sau đó **iSCSI Target** sẽ được đưa vào **iSCSI Controller** để thực thi và xử lý theo yêu cầu. Đến cuối cùng **iSCSI Target** sẽ gửi trả thông tin **iSCSI response**. Từ đó cho phép block data lưu trữ được truyền tải giữa Initiator và Target.

**Note**:
- Các kết nối giữa **Initiator và Target** có thể hoạt động cùng 1 cuộc giao tiếp. Cuộc giao tiếp như vậy được gọi là một **iSCSI session** 

![](https://1.bp.blogspot.com/-QKBDz65Ec-U/WyxkubhLEVI/AAAAAAAAM1Q/2X4D6eupBlMMx8BdOu35zBh71LN9dyLVACLcBGAs/s320/iscsi-session.png)

#### <a name="5"> 5. Một số loại bộ nhớ sao lưu (Backstore) </a>
- **Block** một khối thiết bị lưu trữ được xác định trên server. Nó có thể là disk, partition, logical volume, hoặc bất kỳ files thiết bị nào được chỉ ra trên server.
- **fileio** nó tạo ra một file có kích thước được xác định trong hệ thống file của server.

#### <a name="6"> 6. Cài đặt </a>

https://github.com/letuananh19/thuctapsinh/blob/master/AnhTL/Linux/Ubuntu%20Server/lab/iSCSI.md

## <a name="tltk"> Tài liệu tham khảo: </a>

https://github.com/letuananh19/thuctapsinh/blob/master/NiemDT/Linux/docs/iSCSI.md

http://svuit.vn/threads/ly-thuyet-he-thong-luu-tru-iscsi-san-938/

https://www.server-world.info/en/note?os=Ubuntu_18.04&p=iscsi&f=1

https://www.server-world.info/en/note?os=Ubuntu_18.04&p=iscsi&f=3

http://software.fujitsu.com/jp/manual/manualfiles/m160004/j2ul2107/02enz200/j2107-00-09-06-03.html
