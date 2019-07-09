# iSCSI - Internet Small Computer System Interface

### Mục lục

[1. Cài đặt](#1)

- [1.1: Trên server](#1.1)
- [1.2: Trên Client (Ubuntu)](#1.2)
- [1.3: Trên Client (centOS)](#1.3)

#### <a name="1"> 1. Cài đặt </a>
Ta có mô hình:
![](https://scontent.fhan5-5.fna.fbcdn.net/v/t1.15752-9/66460810_2314449822206897_4691436689166434304_n.png?_nc_cat=101&_nc_oc=AQlxBlZ_NeFbtboJ_S_VmrhBhiuCsN3QWPjIDUzua-RvZGAvwgTKXpm5DijDOPy4TsE&_nc_ht=scontent.fhan5-5.fna&oh=0e873eb591670f17b12d14f134ff6ee7&oe=5DBF7C21)
- Chuẩn bị
  - Trên máy server add thêm một vài ổ cứng.

**<a name="1.1"> 1.1: Trên server (Target) </a>**
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

**<a name="1.2"> 1.2: Trên client (ubuntu) (Initiator) </a>**
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
-- Bỏ note ở dòng 61,62 và chỉ định userid và mật khẩu ta đã đặt trên iSCSI Target.
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
-- Để đăng nhập ta dùng lệnh. Ta có 2 cách để đăng nhập: `` iscsiadm -m node --login`` cho phép ta đăng nhập vào tất cả target tìm thấy ở trên. hoặc ``iscsisdm -m node -T tên_target -p IP`` để đăng nhập vào target của IP ta chỉ định.

![](https://scontent.fhan5-5.fna.fbcdn.net/v/t1.15752-9/66126767_433842577207592_5849087992148262912_n.png?_nc_cat=108&_nc_oc=AQkAY1f2RwEkomihP0vaxQ4haGEo3tq8xwGudvk92Hl7w2VKV5gIh2nJC5QHeAqG0i8&_nc_ht=scontent.fhan5-5.fna&oh=32cca499785a2849dbaa3f80203da103&oe=5DAEADCB)

-- Ta có thể hiển thị phiên kết nối bằng lệnh:
```
iscsiadm -m session -o show
```
-- Hiển thị thông tin node bằng lệnh:
```
iscsiadm -m node -o show
```
-- Dùng lệnh lsblk hiển thị thông tin ổ cứng.
```
lsblk
```

- B5: Để sử dụng nó ta tiến hành phân vùng format và mount nó vaò thư mục và sử dụng bình thường như khi ta gán trực tiếp vào máy của ta.
```
mkfs -t ext4 /dev/sdb1
```
```
mount /dev/sdb1 /trap
```
![](https://scontent.fhan5-2.fna.fbcdn.net/v/t1.15752-9/66135599_1077220865999197_5804684219355496448_n.png?_nc_cat=102&_nc_oc=AQkCCj_Nxw7bWvm1DTIncyrJfRC5ShxIaL-mv6tz21TizSnmuDLpXrW7ncVZYuu6rYo&_nc_ht=scontent.fhan5-2.fna&oh=3376e4c3fcc46ca1640adab95b9a796a&oe=5DB0D8B5)

**<a name="1.3"> 1.3: Trên client (centOS) (Initiator) </a>**
- B1: Cài iscsi
```
yum install iscsi-initiator-utils
```
![]()

- B2: Sửa file `/etc/iscsi/initiatorname.iscsi` để giống với ACL ta tạo trên server để có thể kết nối đến server.

```
vi /etc/iscsi/initiatorname.iscsi
```

![](https://scontent.fhan5-2.fna.fbcdn.net/v/t1.15752-9/66849359_450876339043378_5141649188496343040_n.png?_nc_cat=102&_nc_oc=AQmiJ8FGGj2v9k6x1gAkHQwegCaq68vtqCA_hPGuoOfk7iIR160NA9_xisi-yn0cSZM&_nc_ht=scontent.fhan5-2.fna&oh=c6f2f8f53d63ad95f69c228ebc3510b7&oe=5DC48ABB)

- B3: Vào file ``/etc/iscsi/iscsid.conf``
```
vi /etc/iscsi/iscsid.conf
```
-- Bỏ note ở dòng 57
```
node.session.auth.authmethod = CHAP
```
-- Bỏ note ở dòng 61,62 và chỉ định userid và mật khẩu ta đã đặt trên iSCSI Target.
```
node.session.auth.username = tên_user
node.session.auth.password = mật_khẩu
```

- B4: Sử dụng tiện ích `iscsiadm` để tìm kiếm, đăng nhập, đăng xuất các targets iSCSI.
Để có được danh sách các target trên server ta thực hiện lệnh sau:
```
`iscsiadm -m discovery -t st -p IP_server`
```
Trong ví dụ này server có địa chỉ IP: 192.168.230.142

![](https://scontent.fhan5-7.fna.fbcdn.net/v/t1.15752-9/66481113_616461762209925_1627426141394436096_n.png?_nc_cat=103&_nc_oc=AQkuVO_r_qfB6DM_7MrYo7g7ZvDohXffRM0njay6MDrSYX7zgJDSYVc8tQDyj0SzS7w&_nc_ht=scontent.fhan5-7.fna&oh=79bf2eb94de20998acc22f7d682bb39d&oe=5DB146D3)

Ta có 2 cách để đăng nhập vào Target:
`iscsiadm -m node -l` cho phép ta đăng nhập vào tất cả target tìm thấy ở trên.

`iscsisdm -m node -T tên_target -p IP` để đăng nhập vào target của IP ta chỉ định.

![](https://scontent.fhan5-6.fna.fbcdn.net/v/t1.15752-9/66287857_711017512660489_6425955220156579840_n.png?_nc_cat=107&_nc_oc=AQktT_YJQt7o94AGhSIaW31CaOPHRydKfrdQ-5i_s5bnJ1yacbNcnFVIZ1C6yMbojh0&_nc_ht=scontent.fhan5-6.fna&oh=a5644290d2985ba1ad68585f5abfc55b&oe=5DA78F24)

- B5: Định dạng phân vùng và mount

Ta dùng lệnh ``lsblk`` để kiểm tra xem đã có ổ cứng nào xuất hiện thêm vào chưa.

![](https://scontent.fhan5-3.fna.fbcdn.net/v/t1.15752-9/66112434_383466079190032_6584133926455869440_n.png?_nc_cat=111&_nc_oc=AQkamtlSqqS4KUHGWgdYmFemFaWzDxWKbY_LfUQGBsYnq2I3dFjDrBwfnf7SlB3V0oQ&_nc_ht=scontent.fhan5-3.fna&oh=152880195b83d907410912627fe18906&oe=5DAB3EEB)

Để sử dụng nó ta tiến hành phân vùng format và mount nó vaò thư mục và sử dụng bình thường như khi ta gán trực tiếp vào máy của ta.

![](https://scontent.fhan5-2.fna.fbcdn.net/v/t1.15752-9/66457207_463317397777482_5250398971611840512_n.png?_nc_cat=102&_nc_oc=AQlrNx9TIjc-oOkJhrGpThQo1-W_RuZeTP87JRMQ8Uk-oytDnLp3J7vW27tc2SpSjy4&_nc_ht=scontent.fhan5-2.fna&oh=53fcec67cde7f457ce9cf09ceccc0012&oe=5DB5BAB2)

Để logout khỏi iSCSI targets ta cũng có 2 cách để đăng xuất như khi ta đăng nhập vào. 

`iscsiadm -m node -u` để đăng xuất ra khỏi tất cả các target ta đang đăng nhập.

`iscsiadm -m node -u -T tên_target -p IP` để đăng xuất khỏi một target mà ta chỉ ra.




