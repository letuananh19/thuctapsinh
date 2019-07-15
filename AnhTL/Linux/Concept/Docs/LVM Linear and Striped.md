# LVM linear and striped

### Mục lục:

[1. Khái niệm](#1)

[2. Phân biệt Linear và Striped](#2)

[3. Cài đặt](#3)

------------------

#### <a name="1"> 1. Khái niệm <a/>

- **Linear** và **striped** là tên của hai loại, kiểu ghi dữ liệu vào ổ cứng khi ta dùng kỹ thuật LVM.

#### <a name="2"> 2. Phân biệt Linear và Striped </a>
- **Linear** là cách ghi mặc định, ghi lần lượt lên từng Physical Volume. Khi dữ liệu được ghi đầy lên Physical Volume nó mới thực hiện ghi dữ liệu lên Physical Volume khác.

  - Ví dụ: Ta có một Logical Volume tên là ``LV1`` được hình thành từ 2 Physical Volume là ``/dev/sdb`` và ``/dev/sdc``. Khi ta thực hiện ghi dữ liệu theo cách thông thường vào LV1 thì dữ liệu sẽ được ghi đầy vào ``/dev/sdb`` trước sau đó thì mới ghi vào ``/dev/sdc``.

- **Striped** nó dùng để thay thế cho **Linear**.

  - Ví dụ: Khi ta sử dụng LVM Stripe, dữ liệu ghi vào ``LV1`` sẽ có 50% dung lượng được ghi vào ``/dev/sdb`` và 50% dung lượng ghi vào ``/dev/sdc``. Nói cách khác, khi dùng LVM Stripe thì dữ liệu được ghi vào Logical Volume sẽ được ghi đều lên các Physical Volume tạo lên Logical Volume đó.

![](https://www.smarthomebeginner.com/images/2015/04/Linear-VS-Striped-Volume-500x286.png)

#### <a name="3"> 3. Cài đặt </a>

https://github.com/letuananh19/thuctapsinh/blob/master/AnhTL/Linux/Ubuntu%20Server/lab/Setup%20LVM%20Linear%20and%20Striped%20on%20Ubuntu%20Server.md

# END

## Tài liệu tham khảo:

https://github.com/letuananh19/thuctapsinh/blob/master/NiemDT/Linux/docs/LVM_linear_striped.md

https://github.com/hocchudong/thuctap012017/blob/master/TVBO/docs/LVM/docs/lvm-stripping.md
