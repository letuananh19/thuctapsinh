# Command-line **dd**

### Mục lục:

[1. Khái niệm](#1)

[2. Một số option](#2)

[3. Setup](#3)

-----------------

###  <a name="1"> 1. khái niệm </a>

**dd** là một tiện ích dòng lệnh cho các hệ điều hành giống Unix và giống như Unix với mục đích chính là chuyển đổi và sao chép dữ liệu các tệp.

Trên Linux các ổ đĩa cứng và các thiết bị đặc biệt như ``(/dev/sda và dev/sdb)`` xuất hiện trong hệ thống tệp giống như các tệp thông thường.

**dd** có thể đọc và ghi dữ liệu từ vào các tệp này,

Theo mặc định, **dd** đọc từ stdin và ghi vào thiết bị xuất chuẩn, nhưng chúng có thể được thay đổi bằng cách sử dụng các tùy chọn if (tệp đầu vào) và của (tệp đầu ra).
- stdin viết tắt của ``standard input`` , stdin là một luồng đầu vào nơi dữ liệu được gửi đến và đọc bởi một chương trình.

### <a name="2"> 2. Cấu trúc và một số options </a> 
**2.1: Cấu trúc**
```
dd if=<địa_chỉ_đầu_vào> of=<địa_chỉ_đầu_ra> option
```

Trong đó: “if” đại diện cho input file, và “of” đại diện cho output file  và option

**2.2: option**

- ``bs=Bytes``: quá trình đọc và ghi bao nhiêu byte một lần đọc
- ``cbs=bytes``: Chuyển đổi bao nhiêu byte một lần
- ``count=blocks``: thực hiện bao nhiêu block trong quá trình thực thi câu lệnh
- ``if`` chỉ đường dẫ n đọc đầu vào
- ``of`` chỉ đường dẫn ghi đầu ra
- ``ibs=bytes`` chỉ ra số byte một lần đọc
- ``obs=bytes`` Chỉ ra số byte một lần ghi
- ``skip=blocks`` Bỏ qua bao nhiêu block đầu vào
- ``conv=Convs`` Chỉ ra tác vụ cụ thể của câu lệnh, Các tùy chọn được ghi dưới đây
  - ``ascii``: Chuyển đổi từ mã EBCDIC sang ASCII
  - ``ebcdic``: chuyển đổi từ mã ASCII sang EBCDIC
  - ``lcase`` Chuyển đổi từ chữ thường lên hết thành chứ hoa
  - ``ucase`` Chuyển đổi từ chữ hoa sang chữ thường
  - ``nocreat`` Không tạo ra file đầu ra
  - ``noerror`` tiếp tục sao chép dữ liệu khi đầu vào bị lỗi
  - ``sync`` Đồng bộ dữ liệu với ổ đang sau chép sang
- Note: Một số trường biểu diễn cho số lượng byte
  - ``c`` = 1 byte
  - ``w`` = 2 byte
  - ``b`` = 512 byte
  - ``kB`` = 1000 byte
  - ``K`` = 1024 byte
  - ``MB`` = 1000000 byte
  - ``M`` = 1024*1024 byte
  - ``GB`` = (1000* 1000 *1000) byte
  - ``G`` = (1024 * 1024 * 1024) byte

### <a name="3"> 3. Setup </a>
**Một số ví dụ thực tế**

**3.1: Sao lưu toàn bộ ổ đĩa**: Để sao lưu toàn bộ ổ đĩa này sang ổ đĩa khác được kết nối cùng hệ thống ta thực hiện lệnh sau:

```
dd if=/dev/sdb of=/dev/sdc
```

- Sau khi thực hiện lệnh xong, bản sao chính xác của /dev/sda sẽ có trong /dev/sdb.
- Nếu có bất kỳ lỗi nào xảy ra, lệnh trên sẽ thất bại. Nếu ta cung cấp thêm tham số “conv = noerror” thì sau khỉ xảy ra lỗi nó vẫn sẽ tiếp tục sao chép.
- Ta có thể thêm tùy chọn đồng bộ hóa ``sync``.

```
dd if=/dev/sdb of =/dev/sdc conv = noerror,sync
```

![](https://i.imgur.com/bnjcqEY.png)

**3.2: Sao lưu phân vùng**: 
- Ta có thể sao lưu 1 phân vùng nào đó trong ổ cứng ra 1 thư mục bất kì khác.
```
dd if=/dev/sdb1 of=/test conv=noerror,sync
```

**3.3: Tạo hình ảnh của ổ cứng**: Thay vì sao lưu đĩa cứng, ta có thể tạo tệp hình ảnh của đĩa cứng và lưu nó trong các thiết bị lưu trữ khác. Phương pháp này thường nhanh hơn các loại sao lưu khác, cho phép ta nhanh chóng khôi phục dữ liệu sau khi bất ngờ sảy ra lỗi. Nó tạo ra hình ảnh của đĩa cứng ``/dev/sdb``

```
dd if=/dev/sdb of=~/sdbdisk.img
```

![](https://i.imgur.com/B37CvT3.png)

Nếu muốn nén ảnh file ta có thể sử dụng lệnh dưới đây:

```
dd if=/dev/sdb | grip > /root/sdbdisk.img.gz
```

**3.4: Khôi phục bằng Ảnh ổ cứng**:  Sau khi ta vừa tạo hình ảnh của ổ cứng ta có thể khôi phục bằng lệnh dưới đây:
```
dd if=sdb1disk.img of=/dev/sdc1
```

Tệp hình ảnh ``sdb1disk.img``, là hình ảnh của /dev/sdb1, vì vậy lệnh trên sẽ khôi phục hình ảnh của /dev/sdb1 thành /dev/sdc1.

**3.5: Sao lưu dữ liệu từ môt phân vùng này đến một phân vùng khác**

```
dd if=/dev/sdb1 of=/dev/sdc1 bs=512 conv=noerror,sync
```

![](https://i.imgur.com/88E4i9K.png)

Trong đó: bs=512 có nghĩa là mỗi lần đọc ghi thì nó sẽ đọc 512 byte

**3.6: Chuyển đổi chữ thường thành chữ in hoa**

- Chuyển đổi toàn bộ dữ liệu từ chữ thường thành chữ in hoa

```
dd if=/root/test1.doc of=/root/test2.doc conv=ucase
```

![](https://i.imgur.com/Rl8NjUd.png)

- Chuyển đổi toàn bộ dữ liệu từ chữ hoa thành chữ in thường

```
dd if=/root/test1 of=/root/test2 conv=lcase
```

![](https://i.imgur.com/PnWeAbJ.png)

**3.7: Tạo một file có dung lượng cố định**

- Ví dụ: Tạo 1 file có dung lượng là 1G

```
dd if=/dev/sdb of=/root/file1 bs=1G count=1 
```

![](https://i.imgur.com/PO0z5G5.png)

Có nghĩ là ``/root/file1`` chỉ có dung lượng là 1G. Khi nào ghi đầy 1G nó sẽ không thể ghi thêm nữa.

## END

## Tài Liệu Tham Khào:

https://www.geeksforgeeks.org/dd-command-linux/

https://github.com/duckmak14/thuctapsinh/blob/master/DucNA/liunux/docs/dd.md

http://man7.org/linux/man-pages/man1/dd.1.html
