# Setup LVM Linear and Striped on Ubuntu Server

**LVM Linear**

B1: Tạo logical volume

- Để tạo được ``linear logical volume`` trước tiên ta phải có ``volume group`` và nó phải còn dung lượng còn trống mà chưa cấp cho ``logical volume`` khác. Để tạo ``linear logical volume`` ta sử dụng lệnh:
```
lvcreate --extents N%FREE --name tên_logical tên_group
```

Trong đó:

``N``: là % dung lượng ta định lấy ra trong số dung lượng Volume group còn trống.

``tên_logical`` là tên của logical volume ta định tạo

``tên_group`` là tên của group volume.

Để kiểm tra ta dùng lệnh:
```
lvs
```
or
```
lvdisplay
```

![](https://i.imgur.com/Bl2m678.png)

B2: Ta cần phải format logical volume vừa tạo
```
mkfs -t xfs /dev/vg1/linear
```

![](https://i.imgur.com/fpFPuCt.png)

B3: Để sử dụng nó ta cần phải mount.
```
mount /dev/vg1/linear /test
```

- Ở đây ta mount ``Logical volume Linear``ra thư mục ``test``

![](https://i.imgur.com/A3fq7NY.png)

- Rồi dùng lệnh ``df -h`` để kiểm tra.

![](https://i.imgur.com/gakA0as.png)

B4: Bây giờ ta đã có thể sử dụng được. Ta có thể kiểm tra thông tin của các logical volume và xem nó thuộc kiểu nào dùng Lệnh:
```
lvs --segment
```

![](https://i.imgur.com/DPtXrif.png)

B5: Xem hoạt động của lvm linear.
-  Để làm điều này trước tiên ta cần cài gói ``bwm-ng`` để có thể giám sát sự hoạt động của ổ đĩa
```
sudo apt-get update -y
```
```
sudo apt-get install -y bwm-ng
```

- Ta mở 2 terminal để có thể tiện theo dõi quá trình ghi dữ liệu vào trong thư mục mà ta đã mount ở trên.

Terminal 1: Ta sử dụng lệnh ``dd`` để ghi lên đó 1 file có dung lượng 100M
```
dd if=/dev/sda2 of=/test/text bs=100M count=1
```

![](https://i.imgur.com/KUb5H0g.png)

Terminal 2: Ta sẽ dùng để chạy lệnh theo dõi sự làm việc trên tất cả các physical volume tạo nên linear logical volume.

Để giám sát ta sử dụng lệnh: ``bwm-ng -i disk -I sdb1,sdc1``

![](https://i.imgur.com/C5ZwcEp.png)

Ta có thể thấy dữ liệu được ghi vào ``sdc1``

**LVM Striped**

B1: Ta tạo striped logical volume trước tiên ta cũng phải có volume group và nó phải còn dung lượng còn trống mà chưa cấp cho logical volume khác, ta sử dụng Lệnh:
```
lvcreate --extents N%FREE --stripes X --stripesize Y --name tên_logical tên_group
```
Trong đó: 
``N`` là % dung lượng ta định lấy ra trong số dung lượng Volume group còn trống.
``X`` là số physical volume ta định lấy
``Y`` là dung lượng 1 lần nó ghi trên 1 physical volume(giống vd bên trên)

![](https://i.imgur.com/O4aiBeK.png)

B2: Ta format và mount để sử dụng.

![](blob:https://imgur.com/a2b9df4c-7d3b-490e-8936-2aa8c7c4ce84)

![](https://i.imgur.com/eAamUSP.png)

B3: kiểm tra thông tin của các logical volume và xem nó thuộc kiểu nào ta dùng Lệnh:
```
lvs --segment
```

![](https://i.imgur.com/UM4hXYB.png)

B4: Xem hoạt động của lvm linear.
- Ta mở 2 terminal như ở trên.

Terminal 1: Ta sử dụng lệnh ``dd`` để ghi lên đó 1 file có dung lượng 100M
```
dd if=/dev/sda2 of=/test/text bs=100M count=1
```

![](https://i.imgur.com/ShuCAkX.png)

Terminal 2:  Ta sẽ dùng lệnh ``bwn-ng`` để theo dõi sự làm việc trên tất cả các physical volume tạo nên Striped logical volume.

Để giám sát ta sử dụng lệnh: ``bwm-ng -i disk -I sdb1,sdc1``

Trong đó ``sdb1 và sdc`` là 2 Physical Volume tạo nên Striped logical volume, được ngăn cách nhau bởi dấu phẩy.

![](https://i.imgur.com/THTjM3V.png)

Ta có thể thấy dữ liệu được chia đều cho cả 2 sdb1 và sdc1.

Ta cũng có thể nhận thấy rằng với việc cùng được ghi vào một lượng dữ liệu như nhau thì với ``striped`` ghi nhanh hơn ``linear``

# END

## Tài liệu tham khảo:

https://github.com/letuananh19/thuctapsinh/blob/master/NiemDT/Linux/docs/LVM_linear_striped.md

https://github.com/hocchudong/thuctap012017/blob/master/TVBO/docs/LVM/docs/lvm-stripping.md
