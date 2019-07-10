# LVM on Ubuntu Server 

### Mục lục
[1. Setup LVM](#1)

[2. Thay đổi dung lượng Logical Volume trên LVM](#2)

[3. Thay đổi dung lượng Volume Group trên LVM](#3)

[4. Xóa Logical Volume, Volume Group, Physical Volume](#4)

-------------------

#### <a name="1"> 1. Setup LVM </a>

**Lưu ý: Với centOS ta phải yum install lvm**
```
yum install lvm2
```
  
Ta có mô hình:

![](https://i.imgur.com/AeP35hX.png)

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

![](https://i.imgur.com/OqmF2Cy.png)

- Trong đó:
  - Nhấn ``n`` để bắt đầu tạo partition.
  - Chọn ``p`` để tạo partition primary.
  - Để mặc định theo thứ tự, hoặc chọn số bất kỳ từ 1-4 để tạo partition primary.
  - Tại First sector (2048-20971519, default 2048) ta để mặc định.
  - Tại Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519) ta bấm ``+5G`` để partition tạo ra có dung lượng là 5G.
  - Chọn ``w`` để lưu lại và thoát.

Tiếp theo ta thay đổi định dạng của partition vừa mới tạo thành LVM.

![](https://i.imgur.com/6vJ8cFe.png)

- Trong đó:
- Bấm ``t`` để thay đổi định dạng partition.
- Bấm ``8e`` để đổi thành LVM.
- Bấm ``w`` để lưu.

Tương tự như vậy ta tạo thêm các partition primary của sdb và sdc

![](https://i.imgur.com/Q8bXIo3.png)

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

![](https://i.imgur.com/GanKEPl.png)

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
![](https://i.imgur.com/jA5T4T6.png)

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
![](https://i.imgur.com/NgjSlCe.png)

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

#### <a name="2"> 2. Thay đổi dung lượng Logical Volume trên LVM </a>
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

#### <a name="3"> 3. Thay đổi dung lượng Volume Group trên LVM </a>

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

#### <a name="4"> 4. Xóa Logical Volume, Volume Group, Physical Volume </a>

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
