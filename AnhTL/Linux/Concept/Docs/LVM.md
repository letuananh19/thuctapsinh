# LVM - Logical Volume Manager

![](https://cdn.thegeekdiary.com/wp-content/uploads/2014/10/LVM-basic-structure.png)

# Mục lục

[1. Khái niệm](#1)

- [1.1 LVM là gì](#1.1)
- [1.2 Vai trò của LVM](#1.2)
- [1.3 Các thành phần trong LVM](#1.3)

[2. Setup LVM](#2)

[3. Thay đổi dung lượng Logical Volume trên LVM](#3)

[4. Thay đổi dung lượng Volume Group trên LVM](#4)

[5. Xóa Logical Volume, Volume Group, Physical Volume](#5)

------------

#### <a name="1"> 1. Khái niệm </a>
**<a name="1.1"> 1.1 LVM là gì </a>**
- Logical Volume Manager (LVM): là phương pháp cho phép ấn định không gian đĩa cứng thành những logical Volume khiến cho việc thay đổi kích thước trở nên dễ dàng hơn (so với partition). Với kỹ thuật Logical Volume Manager (LVM) ta có thể thay đổi kích thước mà không cần phải sửa lại table của OS.

**<a name="1.2"> 1.2 Vai trò của LVM </a>**
- LVM là kỹ thuật quản lý việc thay đổi kích thước lưu trữ của ổ cứng

  - Không để hệ thống bị gián đoạn hoạt động
  - Không làm hỏng dịch vụ
  - Có thể kết hợp Hot Swapping (thao tác thay thế nóng các thành phần bên trong máy tính)

**<a name="1.3"> 1.3 Các thành phần trong LVM </a>**

Mô hình các thành phần trong LVM

![](http://www.brainupdaters.net/wp-content/uploads/2017/01/LogicalVolumenManager.jpg)

**Hard drives – Drives**

- Thiết bị lưu trữ dữ liệu, ví dụ như trong linux nó là ``/dev/sda``

**Partition**

- **Partitions** là các phân vùng của Hard drives, mỗi Hard drives có 4 partition, trong đó partition bao gồm 2 loại là ``primary partition`` và ``extended partition``

  - ``Primary partition``
    - Phân vùng chính, có thể khởi động.
    - Mỗi đĩa cứng có thể có tối đa 4 phân vùng này.
  - ``Extended partition``
    - Phân vùng mở rộng.
    - Một phân vùng mở rộng cho phép nhiều hơn bốn ổ đĩa trên cùng một đĩa vật lý.
![](https://www.pcmag.com/encyclopedia_images/PARTITE.GIF)

**Physical Volumes**

- Là một cách gọi khác của partition trong kỹ thuật LVM, nó là những thành phần cơ bản được sử dụng bởi LVM. Một Physical Volume không thể mở rộng ra ngoài phạm vi một ổ đĩa.
- Chúng ta có thể kết hợp nhiều Physical Volume thành Volume Groups.

**Volume Group**

- Nhiều Physical Volume trên những ổ đĩa khác nhau được kết hợp lại thành một Volume Group
![](https://bachkhoa-aptech.edu.vn/upload/image/gioi-thieu-ve-logical-volume-manager-02.png)

- Volume Group được sử dụng để tạo ra các Logical Volume, trong đó người dùng có thể tạo, thay đổi kích thước, lưu trữ, gỡ bỏ và sử dụng.

  - ``Một điểm cần lưu ý là boot loader không thể đọc /boot khi nó nằm trên Logical Volume Group. Do đó không thể sử dụng kỹ thuật LVM với /boot mount point``

**Logical Volume**

- Volume Group được chia nhỏ thành nhiều Logical Volume, mỗi Logical Volume có ý nghĩa tương tự như partition. Nó được dùng cho các mount point và được format với những định dạng khác nhau như ext2, ext3, ext4,…

![](https://d2908q01vomqb2.cloudfront.net/887309d048beef83ad3eabf2a79a64a389ab1c9f/2018/08/15/ebs-elastic-2-1.gif)

- Khi dung lượng của Logical Volume được sử dụng hết ta có thể đưa thêm ổ đĩa mới bổ sung cho Volume Group và do đó tăng được dung lượng của Logical Volume

  - Ví dụ ta có 4 ổ đĩa mỗi ổ 5GB khi ta kết hợp nó lại thành 1 volume group 20GB, và ta có thể tạo ra 2 logical volumes mỗi disk 10GB

**File Systems**

- Tổ chức và kiểm soát các tập tin.
- Được lưu trữ trên ổ đĩa cho phép truy cập nhanh chóng và an toàn.
- Sắp xếp dữ liệu trên đĩa cứng máy tính.
- Quản lý vị trí vật lý của mọi thành phần dữ liệu.

#### <a name="2"> 2. Setup LVM </a>
  
- Ta sẽ thực hành trên ubuntu server 18.04
- Trên máy Ubuntu này đã thêm 3 ổ cứng - mỗi ổ 20G

**B1: Kiểm tra các Hard Drives đã có trên hệ thống**

Kiểm tra xem có những Hard Drives nào trên hệ thống bằng cách sử dụng câu lệnh ``lsblk``
```
lsblk
```
![](https://scontent.fhan5-6.fna.fbcdn.net/v/t1.15752-9/66448818_704848076641569_7140351205108088832_n.png?_nc_cat=105&_nc_oc=AQk7rdHa8uCdW-5gh6CuAqmontlIAP_gEDs0eTDX4nLPZ1VQjSViomnTXY0CBnH-fI4&_nc_ht=scontent.fhan5-6.fna&oh=b182acbe9decb052363125dee91a728a&oe=5DB8FDB3)

Ta có thể thấy có 3 ổ là ``sdb, sdc, sdd`` là các ổ ta đã thêm vào. Còn ổ ``sda`` là ổ dành cho OS nên Không thể tạo được.

**B2: Tạo Partition**

Từ các Hard Drives trên hệ thống, ta tạo các partition. Ở đây, từ sdb, mình tạo các partition bằng cách sử dụng lệnh sau ``fdisk /dev/sdb``
```
fdisk /dev/sdb
```

![](https://i.imgur.com/YfsAr3s.png)

- Trong đó:
  - Nhấn ``n`` để bắt đầu tạo partition.
  - Chọn ``p`` để tạo partition primary.
  - Để mặc định theo thứ tự, hoặc chọn số bất kỳ từ 1-4 để tạo partition primary.
  - Tại First sector (2048-20971519, default 2048) ta để mặc định.
  - Tại Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519) ta bấm ``+5G`` để partition tạo ra có dung lượng là 5G.
  - Chọn ``w`` để lưu lại và thoát.

Tiếp theo ta thay đổi định dạng của partition vừa mới tạo thành LVM.

![](https://i.imgur.com/ynFznGU.png)

- Trong đó:
- Bấm ``t`` để thay đổi định dạng partition.
- Bấm ``8e`` để đổi thành LVM.
- Bấm ``w`` để lưu.

Tương tự như vậy ta tạo thêm các partition primary của sdb và sdc

![](https://i.imgur.com/zZfz6Xa.png)

**B3: Tạo Physical Volume**

Tạo các Physical Volume là /dev/sdb1 và /dev/sdc1 bằng các lệnh sau:
```
pvcreate /dev/sdb1
```
```
pvcreate /dev/sdc1
```

Ta có thể kiểm tra các Physical Volume bằng lệnh ``pvs`` hoặc `` pvdisplay``
```
pvs
```
or
```
pvdisplay
```

![](https://i.imgur.com/ku9mWBD.png)

**B4: Tạo Volume Group**

Tiếp theo, ta sẽ nhóm các Physical Volume thành 1 Volume Group bằng cách sử dụng câu lệnh sau:
```
vgcreate vg1 /dev/sdb1 /dev/sdc1
```
Trong đó ``vg1`` là tên của Volume Group.

Sử dụng câu lệnh sau để kiểm tra lại các Volume Group đã tạo:
```
vgs
```
or
```
vgdisplay
```
![](https://i.imgur.com/91LQZHu.png)

**B5: Tạo Logical Volume**

Từ một Volume Group, giờ ta có thể tạo ra các Logical Volume bằng cách sử dụng lệnh sau:
```
lvcreate -L 5G -n lv1 vg1
```
- Trong đó:
  - ``-L``: Chỉ ra dung lượng của logical volume.
  - ``-n``: Chỉ ra tên của logical volume.
  -  ``lv1`` là tên Logical Volume.
  -  ``vg1`` là Volume Group mà mình vừa tạo ở bước trước.
     - Lưu ý là ta có thể tạo nhiều Logical Volume từ 1 Volume Group

Sử dụng câu lệnh sau để kiểm tra lại các Logical Volume đã tạo:
```
lvs
```
or
```
lvdisplay
```
![](https://i.imgur.com/EuQpPCt.png)

**B6: Định dạng Logical Volume**

Để format các Logical Volume thành các định dạng như ext2, ext3, ext4, ta có thể làm như sau:
```
mkfs -t ext4 /dev/vg1/lv1
```

![](https://i.imgur.com/rzFpCwN.png)

**B7. Mount và sử dụng**

Giờ ta sẽ mount về thư mục mà ta muốn và có thể sử dụng được.
```
mkdir lvm
```
Ta sẽ tiến hành ``mount`` logical volume lv1 vào thư mục lvm như sau:
```
mount /dev/vg1/lv1 lvm/
```
Và dùng lệnh df -h để kiểm tra lại dung lượng của thư mục đã được mount:
```
df -h
```
![](https://i.imgur.com/1IRs1bE.png)

#### <a name="3"> 3. Thay đổi dung lượng Logical Volume trên LVM </a>
Phần trước ta đã tiến hành tạo Logical Volume trong LVM.

Ở phần này, ta sẽ tìm hiểu làm thế nào để có thể thay đổi dung lượng của 1 Logical Volume đã được tạo ở phần trước.

Trước khi thay đổi dung lượng, ta cần phải kiểm tra các thông tin hiện có bằng lệnh:
```
pvs
```
```
vgs
```
```
lvs
```

![](https://i.imgur.com/tYg441G.png)

Ở đây, ta đã tạo được Logical Volume  có tên là ``lv1``, và giả sử Logical Volume này dung lượng đã đầy và ta cần tăng kích thước của nó để tiếp tục sử dụng..

Logical Volume này thuộc Volume Group có tên vg1, để tăng kích thước, bước đầu tiên phải kiểm tra xem Volume Group còn dư dung lượng để kéo giãn Logical Volume không. Logical Volume thuộc 1 Volume Group nhất định, Volume Group đã cấp phát hết thì Logical Volume cũng không thể tăng dung lượng được. Để kiểm tra, ta dùng lệnh:
```
vgs
```

![](https://i.imgur.com/q16wfzR.png)

Volume Group ở đây vẫn còn dung lượng để cấp phát, ta có thể nhận thấy điều này qua 2 trường thông tin là ``Vsize`` và ``VFree`` với dung lượng Free là ``4.99g``/``9.99g``

Để tăng kích thước Logical Volume ta sử dụng câu lệnh sau:
```
lvextend -L +100M /dev/vg1/lv1
```

Với ``-L`` là tùy chọn để tăng kích thước.

![](https://i.imgur.com/muf3U7w.png)

Kiểm tra lại bằng cách dùng lệnh: 
```
lvs
```

![](https://i.imgur.com/XiGYXK5.png)

So sánh trước và sau khi tăng lên ta có thể xem hình dưới đây:
![](https://i.imgur.com/E3TncDQ.png)

Sau khi tăng kích thước cho Logical Volume thì Logical Volume đã được tăng nhưng file system trên volume này vẫn chưa thay đổi, bạn phải sử dụng lệnh sau để thay đổi:
```
resize2fs /dev/vg1/lv1
```

![](https://i.imgur.com/8ELs81U.png)

Để giảm kích thước của Logical Volume, trước hết ta phải umount Logical Volume mà mình muốn giảm
```
umount /dev/vg1/lv1
```

Tiến hành giảm kích thước của Logical Volume
```
lvreduce -L 1G /dev/vg1/lv1
```

Rồi dùng lệnh ``lvs`` để kiểm tra logical volume
![](https://i.imgur.com/PnLZciK.png)

Trước khi giảm thì tổng dung lượng của logical volume là 5.10g, sau khi giảm thì chỉ còn 1.00g

Sau đó tiến hành format lại Logical Volume
```
mkfs.ext4 /dev/vg1/lv1
```

Cuối cùng là mount lại Logical Volume
```
mount /dev/vg1/lv1 lvm
```

![](https://i.imgur.com/6BORs9W.png)

#### <a name="4"> 4. Thay đổi dung lượng Volume Group trên LVM </a>

Ở phần ta tăng kích thước của Logical Volume nhưng với điều kiện Volume Group của Logical Volume đó còn dung lượng. 

Phần này chúng ta sẽ tìm hiểu xem làm thế nào có thể mở rộng thêm kích thước của Volume Group cũng như thu hồi dung lượng của nó.

Việc thay đổi kích thước của Volume Group chính là việc nhóm thêm Physical Volume hay thu hồi Physical Volume ra khỏi Volume Group.

Trước tiên, ta cần kiểm tra lại các partition và Volume Group:
```
vgs
```
```
lsblk
```

![](https://i.imgur.com/SKS8ovr.png)

Tiếp theo, ta thêm partition ``sdb2`` vào Volume Group như sau:
```
vgextend /dev/vg1 /dev/sdb2
```

![](https://i.imgur.com/0FWP5RJ.png)

Ở đây, muốn nhóm vào Volume Group phải là Physical Volume nên hệ thống đã tự động tạo cho mình Physical Volume và tự nhóm vào Volume Group.

Bỏ 1 Physical Volume ra khỏi Volume Group như sau:
```
vgreduce /dev/vg1 /dev/sdb2
```

![](https://i.imgur.com/cIspW1z.png)

#### <a name="5"> 5. Xóa Logical Volume, Volume Group, Physical Volume </a>

**Xóa Logical Volumes**

Trước tiên ta phải ``Umount`` Logical Volume
```
umount /dev/vg1/lv1
```

Sau đó ta tiến hành xóa Logical Volume bằng lệnh sau:
```
lvremove /dev/vg1/lv1
```
Ta kiểm tra lại kết quả:

![](https://i.imgur.com/j9AEqAf.png)

**Xóa Volume Group**

Trước khi xóa Volume Group, ta phải xóa Logical Volume

Xóa Volume Group bằng cách sử dụng lệnh sau:
```
vgremove /dev/vg1
```

![](https://i.imgur.com/JgblVg9.png)

**Xóa Physical Volume**

Cuối cùng là xóa Physical Volume:
```
pvremove /dev/sdb2
```

![](https://i.imgur.com/97Oipyw.png)

# END!

# Tài liệu tham khảo

https://bachkhoa-aptech.edu.vn/gioi-thieu-ve-logical-volume-manager.html

https://github.com/duckmak14/linux/blob/master/finish/LVM.md
