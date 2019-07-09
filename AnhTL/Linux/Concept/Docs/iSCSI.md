# iSCSI - Internet Small Computer System Interface

![](https://community.fs.com/blog/wp-content/uploads/2018/08/how-iSCSI-storage-works.jpg)

#### 1. Khái niệm
**iSCSI** được xem là một giao thức để truyền tải các SCSI qua mạng IP bằng giao thức TCP/IP. Từ đó iSCSI cho phép truy cập các khối dữ liệu (phân vùng, disk, hoặc thiết bị lưu trữ) trên server qua các lệnh SCSI và truyền tải dữ liệu trên hệ thống mạng(LAN/WAN).

#### 2. Các thành phần của iSCSI
Một kết nối iSCSI sẽ chia ra làm 2 thành phần chính.
- **iSCSI Initiator**
- **iSCSI Target**

![](https://cuongquach.com/resources/images/2018/01/initiator-target-iscsi.png)

**2.1 iSCSI Initiator**
Là máy client. Nó gửi yêu cầu truy cập đến server để truy cập vào khối dữ liệu và truyền tải các lệnh iSCSI qua đường truyền mạng TCP/IP.

**2.2 iSCSI Target**
Đóng vai trò server, nơi lưu trữ dữ liệu. Mỗi máy target phải có một tên duy nhất để các initiator truy cập vào. Một target có một hoặc nhiều hơn 1 khối các thiết bị lưu trữ.

Từ máy chủ **iSCSI Target** này sẽ tiếp nhận các request từ **iSCSI Initiator** gửi đến và trả dữ liệu về 

Trên **iSCSI Target** sẽ quản lý ổ đĩa iSCSI với các tên gọi **LUN (Logical Unit Number)** được dùng để chia sẻ ổ đĩa lưu trữ iSCSI với phía iSCSI Client.

#### 3. Một số thuật ngữ trong iSCSI
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

#### 4. iSCSI hoạt động như thế nào?

![](https://cuongquach.com/resources/images/2018/01/iscsi-communication-1.png)

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

#### 5. Một số loại bộ nhớ sao lưu (Backstore)
- **Block** một khối thiết bị lưu trữ được xác định trên server. Nó có thể là disk, partition, logical volume, hoặc bất kỳ files thiết bị nào được chỉ ra trên server.
- **fileio** nó tạo ra một file có kích thước được xác định trong hệ thống file của server.

#### 6. Cài đặt 
Ta có mô hình:
![](https://scontent.fhan5-2.fna.fbcdn.net/v/t1.15752-9/66328641_462798444497366_4647123570248384512_n.png?_nc_cat=110&_nc_oc=AQlX_p3yo4uTgm7QO3gypk8Pe56g3Za2SlqerilwX81jv9x9ptC32-fh0ZEhtWf9Pyc&_nc_ht=scontent.fhan5-2.fna&oh=11a2052e1aed61842652834b36e7d9f8&oe=5DA50D99)
- Chuẩn bị
  - Ít nhất 2 máy, 1 máy làm server store, 1 máy làm client
  - Trên máy server add thêm một vài ổ cứng.

**6.1: Trên server (Target)**
- B1: Ta cài đặt Target
```
apt -y install targetcli-fb
```
- B2: Ta dùng lệnh ``targetcli`` để hiện dấu nhắc tương tác với iSCSI.
```
targetcli
```
![](https://scontent.fhan5-7.fna.fbcdn.net/v/t1.15752-9/66134188_353181535360365_412282200029921280_n.png?_nc_cat=103&_nc_oc=AQmNxJKdOVjDkq8CyN0w3MJWMokJhMH-rc-kChTCY8bA4c9ckPVi6Mma3JQvxoUObBM&_nc_ht=scontent.fhan5-7.fna&oh=a5a25eb0c6091a9aca803e3f8c931431&oe=5DBF4D70)

- B3: Ta tiến hành cd vào thư mục ``backstores/block`` hoặc ``backstores/fileio`` tùy theo ta muốn tạo backstores theo kiểu nào.
```
cd backstores/block
```
```
cd backstores/fileio
```
![](https://scontent.fhan5-4.fna.fbcdn.net/v/t1.15752-9/64929825_2323161691278392_508341617506123776_n.png?_nc_cat=104&_nc_oc=AQn0cI4lNaP5a3kjDLtg2AwVKyiCtWc7bDIfij_Gl9epmN2zhovuOrqnH4rm7NX4ZUg&_nc_ht=scontent.fhan5-4.fna&oh=f172708882e71511c0dd524773d8aef5&oe=5DADF388)

Như vậy là ta đã có thể tạo một fileio(có thể là block) và chỉ định cho nó dùng thiết bị lưu trữ nào (có thể là disk, partition hoặc là logical volume). Cú pháp ``create tên_fileio thiêt_bị`` Ở đây ta để tên fileio là disk01 còn thiết bị gán cho nó là ổ /dev/sdb (mặc định sẽ là toàn bộ G của đĩa, hoặc ta có thể chỉ định G cho nó)
```
create disk01 /dev/sdb
```

- B4: Ta tạo /iscsi target. Ở đây ta phải sử dụng tên theo tiêu chuẩn (đã mô tả ở trên). Tên miền ở đây là ``nhanhoa.com.vn`` nhưng trong quy định về đặt tên thì tên miền phải được viết đảo ngược lại.
```
cd /iscsi
```
```
create iqn.2019-07.vn.com.nhanhoa:disk1
```
![](https://scontent.fhan5-4.fna.fbcdn.net/v/t1.15752-9/66284931_506199850117217_6427975508348108800_n.png?_nc_cat=104&_nc_oc=AQnhUp35rBBiBA3gtZjYkmTplorjT35Sx2qw4WPFS8nR5kw_hM89g-vwDobdlBeEc6k&_nc_ht=scontent.fhan5-4.fna&oh=9c08fce0d9724cf2cb55bf0b6cc87616&oe=5DADF3BB)

- B5: Tạo ``LUN`` dưới target. ``LUN`` nên sử dụng đối tượng lưu trữ sao lưu được đề cập trước đây ``disk01`` hoặc ta cũng có thể chỉ ra các đường dẫn tới các thiết bị khác đã được tạo
```
cd iqn.2019-07.vn.com.nhanhoa:disk1/tgp1/luns
```
```
create /backstores/fileio/disk01
```
![](https://scontent.fhan5-7.fna.fbcdn.net/v/t1.15752-9/66934744_504847776921084_7784329582039007232_n.png?_nc_cat=100&_nc_oc=AQkN0v2wom7cl03mEZPuxqKejngzLPp1FjTMG0HzaBZGnHDkGyYs-wEBJj0MEEjtjJ4&_nc_ht=scontent.fhan5-7.fna&oh=f4a4e7cb6bf1cbee9be394240db050b0&oe=5DA633D7)

- B6: Tạo ACL để khi client trình bày đúng cái tên này thì nó mới được target chấp nhận kết nối. 
```
cd ../acls
```
```
create iqn.2019-07.vn.com.nhanhoa:123456
```
![](https://scontent.fhan5-4.fna.fbcdn.net/v/t1.15752-9/66493084_389548975099535_5204312705510408192_n.png?_nc_cat=104&_nc_oc=AQkzw-tdetMKY0vQ0ZM3ZcssVqgpm9WvnrPNNSVbbAHoUl12A4sMON467a0pUXj8j5c&_nc_ht=scontent.fhan5-4.fna&oh=f02607e6fbb61f6499bf327e517d31a7&oe=5DA9A358)

-- 1 là: là phần tên máy nó cố định như ta đặt từ trước
-- 2 là: password đăng nhập.

`` LUN 0 `` ta vừa tạo bên trên đã được map vào ACL ta vừa tạo.

- B7: Ta set ID người dùng để xác thực
```
cd iqn.2019-07.vn.com.nhanhoa:123456
```
```
set auth userid=tên_user
```
```
set auth password=password
```

![](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/66697436_385712575412126_5477600027150909440_n.png?_nc_cat=111&_nc_oc=AQk9Du_zMLXyCWar--Jl2aBDj3x00GdmzRn5innCl_a1kYWC_W8OFCUU3WFVF__-Okk&_nc_ht=scontent.fhan5-3.fna&oh=893b18dd2d14dca387819e9810d59736&oe=5DBAB0F1)

- B8: Ta lưu nó lại
```
saveconfig
```
-- sau đó exit
```
exit
```
![](https://scontent.fhan5-4.fna.fbcdn.net/v/t1.15752-9/66738871_754579934958496_6408055484388999168_n.png?_nc_cat=104&_nc_oc=AQlb16ZrlMCLPaR0jPnsHlCsWUMPVyktxk3Jpf-9V6erwUVdpNk4YZSHjCzK2RW33mg&_nc_ht=scontent.fhan5-4.fna&oh=4d0df0b98fa2f81a66b065635f38eba8&oe=5DAEFBFD)

- B9: Tắt filewall để client có thể truy cập vào
```
ufw disable
```
```
ufw allow 3260
```
![](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/66105913_1115832385269281_1302387749487116288_n.png?_nc_cat=111&_nc_oc=AQkZj_yrvpGZhluB-RVEeN2MV-RUCnvG-uHnGvdt_FBb1aI1xKLcS1Nr6RR0oPDNn9U&_nc_ht=scontent.fhan5-3.fna&oh=36b4a026e5df2643cc48e60b6a31c5a7&oe=5D7C855B)

**6.2: Trên client (Initiator)**
- B1: Cài iSCSI
```
apt -y install open-iscsi
```

- B2: Ta thay đổi iqn trong file ``/etc/iscsi/initiatorname.iscsi``
```
vi /etc/iscsi/initiatorname.iscsi
```
-- Thay đổi thành ALC giống như ta đã đặt trên máy chủ iSCSI Target thì mới kết nối được.
```
iqn.2019-07.vn.com.nhanhoa:123456
```
![](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/66503308_1598036266994794_8434440339044958208_n.png?_nc_cat=106&_nc_oc=AQnNF1Zuu7hV2PMe3_8YT-QDkrV7pva-zFkiLNL9-JJoh3xx9Eb8JwTcUoIxFx37Lpg&_nc_ht=scontent.fhan5-3.fna&oh=4fb1c691fadc0c01a3261c1e3ae26516&oe=5DAFFA46)

- B3: Ta vào file ``/etc/iscsi/iscsid.conf``

-- Bỏ note ở dòng 57
```
node.session.auth.authmethod = CHAP
```
-- Bỏ note ở dòng 61,62 và chỉ định userid và mật khẩu ta đã đặt trên iSCSI Target để yêu cầu client phải đăng nhập vào Target
```
node.session.auth.username = tên_user
node.session.auth.password = mật_khẩu
```
![](https://scontent.fhan5-7.fna.fbcdn.net/v/t1.15752-9/66112128_591207051406214_6676707333726273536_n.png?_nc_cat=100&_nc_oc=AQkMYZ49Peomr7EJ85ds6HdohPmKGsdX_4m7RELHRtEHhcpa09M9G0EwHoYaEPAF7ss&_nc_ht=scontent.fhan5-7.fna&oh=dc2412bd61624666ddacc5ea6cf6b880&oe=5DB16689)

-- Sau đó ta restart iscsi
```
systemctl restart iscsid open-iscsi
```

- B4: Sử dụng tiện ích iscsiadm để tìm kiếm, đăng nhập, đăng xuất các targets iSCSI. 

Để có được danh sách các target trên server ta thực hiện cú pháp sau: ``iscsiadm -m discovery -t st -p IP_server`` Trong ví dụ này server có địa chỉ IP: 192.168.230.142
```
iscsiadm -m discovery -t sendtargets -p 192.168.230.142
```
-- Để đăng nhập ta dùng lệnh. Ta có 2 cách để đăng nhập: `` iscsiadm -m node -l`` cho phép ta đăng nhập vào tất cả target tìm thấy ở trên. hoặc ``iscsisdm -m node -T tên_target -p IP`` để đăng nhập vào target của IP ta chỉ định.
