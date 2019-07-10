# LVM - Logical Volume Manager

![](https://cdn.thegeekdiary.com/wp-content/uploads/2014/10/LVM-basic-structure.png)

# Mục lục

[1. Khái niệm](#1)

- [1.1 LVM là gì](#1.1)
- [1.2 Vai trò của LVM](#1.2)
- [1.3 Các thành phần trong LVM](#1.3)

[2. Setup LVM](#2)


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

https://github.com/letuananh19/thuctapsinh/blob/master/AnhTL/Linux/Ubuntu%20Server/lab/LVM.md

# END!

# Tài liệu tham khảo

https://bachkhoa-aptech.edu.vn/gioi-thieu-ve-logical-volume-manager.html

https://github.com/duckmak14/linux/blob/master/finish/LVM.md
