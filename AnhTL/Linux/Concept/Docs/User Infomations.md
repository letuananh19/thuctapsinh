## File /etc/passwd

**Khái niệm**

- ``/etc/passwd`` là tập tin lưu trữ thông tin cần thiết, được yêu cầu trong quá trình đăng nhập tức là thông tin tài khoản người dùng. 
- ``/etc/passwd`` là một tệp văn bản, chứa danh sách các tài khoản của hệ thống, cung cấp cho mỗi tài khoản một số thông tin hữu ích như ID user, ID group, thư mục chính, shell, v.v.

**Các thuật ngữ trong /etc/passwd**

- ``/etc/passwd`` chứa một mục nhập trên mỗi dòng cho mỗi người dùng (hoặc tài khoản người dùng) của hệ thống. Tất cả các trường được phân tách bằng ký hiệu dấu hai chấm (:). Tổng cộng có bảy lĩnh vực trên 1 user.

![](https://www.cyberciti.biz/media/ssb.images/uploaded_images/passwd-file-791527.png)

Trong đó:

- **1: Tên người dùng**: Nó được sử dụng khi người dùng đăng nhập. Nó phải có độ dài từ 1 đến 32 ký tự.
- **2: Mật khẩu** : Một ký tự ``x`` chỉ ra rằng mật khẩu được mã hóa được lưu trữ trong tập tin /etc/shadow
- **3: ID người dùng (UID)** : Mỗi người dùng phải được chỉ định một ID người dùng (UID). UID ``0 (không)`` được dành riêng cho root và UID 1-99 được dành riêng cho các tài khoản được xác định trước khác. Hơn nữa UID 100-999 được hệ thống dành riêng cho các nhóm / tài khoản quản trị và hệ thống.
- **4: ID nhóm (GID)** : Đây là ID group của user (được lưu trữ trong /etc/group).
- **5: Thông tin ID người dùng** : Trường này cho phép ta thêm thông tin bổ sung về người dùng như họ tên của người dùng, số điện thoại, v.v. Trường này sử dụng bằng lệnh.
- **6: Thư mục chính** : Đường dẫn tuyệt đối đến thư mục người dùng sẽ vào khi họ đăng nhập. Nếu thư mục này không tồn tại thì thư mục người dùng sẽ trở thành /
- **7: Lệnh/shell** : Đường dẫn tuyệt đối của lệnh hoặc shell (/bin/bash). Tương ứng với user đang sử dụng ``shell`` nào 

Mật Khẩu của user đã được mã hóa và lưu trong ``/etc/shadow``

------------

## File /etc/shadow

**Khái niệm**

- ``/etc/passwd`` là tập tin lưu trữ thông tin mật khẩu của người dùng
- Tệp /etc/Shadow có chín trường để lưu mật khẩu được mã hóa và các thông tin liên quan đến mật khẩu khác.
- Tệp /etc/Shadow hỗ trợ tất cả các thuật toán nâng cao và có nhiều chỗ để cập nhật thêm.
- Tập tin /etc/Shadow chỉ có thể đọc được bởi người dùng root và super user.

**Các thuật ngữ trong /etc/passwd**

- Mỗi dòng trong tập tin /etc/shadow đại diện cho một tài khoản người dùng  và chứa sau 9 lĩnh vực phân cách bằng dấu hai chấm (:).

![](https://i.imgur.com/cjbYpzT.png)

  - 1: Tên user.
  - 2: Mật khẩu được mã hóa.
  - 3: Ngày thay đổi mật khẩu cuối cùng.
  - 4: Ngày bắt buộc tối thiểu giữa các lần thay đổi mật khẩu.
  - 5: Ngày tối đa được phép giữa các lần thay đổi mật khẩu.
  - 6: Số ngày trước để hiển thị thông báo hết hạn mật khẩu.
  - 7: Số ngày sau khi hết hạn mật khẩu để vô hiệu hóa tài khoản.
  - 8: Ngày hết hạn tài khoản.
  - 9: Lĩnh vực dự trữ.

![](https://www.computernetworkingnotes.org/images/linux/rhce-study-guide/rsg03-02-etc-shadow-file.png)

- 1: Tên user.
  - Ngoại trừ thông tin mật khẩu, tất cả thông tin đăng nhập khác được lưu trữ trong tập tin /etc/passwd . Trường này kết nối /etc/shadow với tập tin /etc/passwd . Trong cả hai tệp, trường này đại diện cho tên đăng nhập và lưu trữ thông tin chính xác như nhau. Khi một tài khoản người dùng mới được tạo, cả hai tệp được cập nhật đồng thời.

- 2: Mật khẩu được mã hóa.

  - Trường này lưu trữ mật khẩu người dùng thực tế ở dạng mã hóa. Để mã hóa, nó sử dụng thuật toán SHA512. Trong thuật toán này, một chuỗi ký tự ngẫu nhiên được trộn với mật khẩu gốc trước khi mã hóa. Nếu hai hoặc nhiều người dùng đã chọn cùng một mật khẩu, do tính năng này, mật khẩu được mã hóa của họ sẽ khác nhau.
  
  - Kiểm soát đăng nhập.
  
     - Linux không hỗ trợ mật khẩu trống trong quá trình đăng nhập. Bất kỳ người dùng hoặc dịch vụ nào không có mật khẩu hợp lệ hoặc có mật khẩu trống đều không được phép đăng nhập. Bằng cách đặt giá trị khác với mật khẩu được mã hóa, trường này có thể được sử dụng để kiểm soát đăng nhập của người dùng. Ví dụ: nếu giá trị ( ! ) Hoặc ( * ) được lưu trữ trong trường này, tài khoản sẽ bị khóa và người dùng hoặc dịch vụ sẽ không được phép đăng nhập.

     - Cả hai ký tự ( ! Và * ) đại diện cho một mật khẩu trống. Sự khác biệt giữa cả hai ký tự là, ký tự đầu tiên, dấu chấm than ( ! ), Được sử dụng cho tài khoản người dùng và ký tự thứ hai, dấu hoa thị ( * ), được sử dụng cho tài khoản dịch vụ. Nếu được yêu cầu, tài khoản người dùng có thể được mở khóa bằng cách đặt mật khẩu trong trường này thông qua lệnh ``passwd``.

![](https://i.imgur.com/kPqAdE0.png)

- 3: Ngày thay đổi mật khẩu cuối cùng

  - Trường này sẽ ghi lại số ngày kể từ khi mật khẩu của người dùng được thay đổi lần cuối. Để tính số ngày, nó sử dụng ngày 1 tháng 1 năm 1970 làm ngày bắt đầu. 
     - VD: một user đã thay đổi mật khẩu vào ngày 25 tháng 6 năm 2018 thì số ngày sẽ là 17707.
  - Trong Linux, ngày 1 tháng 1 năm 1970 được gọi là kỷ nguyên. Ngày này được sử dụng làm ngày bắt đầu hoặc ngày được tính toán bằng một số lệnh và tệp cấu hình.
     - Lý do: Hệ điều hành Unix được ``Ken Thompson`` và ``Dennis Ritchie`` (cả hai thuộc AT&T Bell Laboratories) xây dựng và phát hành lần đầu tiên vào năm 1970.
     
![](https://www.computernetworkingnotes.org/images/linux/rhce-study-guide/rsg03-04-start-conting.png)

  - Ta có thể dùng lệnh dưới đây để hiển thị ra ngày thay đổi mật khẩu.
  ```
  date -d "1970-01-01 [số ngày] days"
  ```
  ![](https://i.imgur.com/wjCpRDa.png)
      [ Số ngày ] ta có thể kiếm tra trong file /etc/shadow 

- 4: Ngày bắt buộc tối thiểu giữa các lần thay đổi mật khẩu.
  - Khi mật khẩu được thay đổi, người dùng không được phép thay đổi mật khẩu cho đến khi hết ngày được chỉ định trong trường này. Nếu giá trị được đặt thành 0 ( không ), người dùng được phép thay đổi mật khẩu của mình ngay lập tức. (mặc định là 0)

- 5: Ngày tối đa được phép giữa các lần thay đổi mật khẩu.
  - Tệp này đặt số ngày tối đa được ta phải thay đổi mật khẩu. User phải thay đổi mật khẩu trước khi hết hạn ngày được đặt. Nếu trường này được đặt thành trống (0), người dùng có thể sử dụng mật khẩu của mình bất cứ lúc nào. Mặc định sẽ là 99999

- 6: Số ngày trước để hiển thị thông báo hết hạn mật khẩu.
  - Trường này đặt số ngày trước để hiển thị thông báo hết hạn mật khẩu. Có nghĩa là đến ngày 99992 người dùng sẽ nhận được một thông báo cảnh báo để thay đổi mật khẩu của mình.

     - Thông báo cảnh báo sẽ chỉ được hiển thị khi người dùng sẽ đăng nhập vào thiết bị đầu cuối dòng lệnh. Thông báo này sẽ không được hiển thị nếu người dùng đăng nhập vào màn hình GUI.

- 7: Số ngày sau khi hết hạn mật khẩu để vô hiệu hóa tài khoản
  - Trường này đặt số ngày sau khi hết hạn mật khẩu để vô hiệu hóa tài khoản. Nếu người dùng không thay đổi mật khẩu trong những ngày tối đa được phép, mật khẩu của user sẽ được đánh dấu là hết hạn. Tài khoản user hết hạn mật khẩu sẽ tự động bị vô hiệu hóa sau khi hết ngày được chỉ định trong trường này. Mặc định là không có.

- 8: Ngày hết hạn tài khoản.
  - Trường này đặt ngày hết hạn tài khoản. Người dùng không được phép đăng nhập sau ngày được chỉ định trong trường này. Để chỉ định ngày, số ngày bắt đầu từ ngày 1 tháng 1 năm 1970 được sử dụng. 
     - Ví dụ: để đặt ngày hết hạn tài khoản đến ngày 28 tháng 6 năm 2018, số 17710 sẽ được sử dụng. Nếu trường này được đặt thành trống, tài khoản người dùng sẽ không bao giờ hết hạn.

- 9: Lĩnh vực dự trữ.
  - Trường này vẫn đang trong quá trình nghiên cứu, đây trường dành riêng và không lưu trữ bất kỳ giá trị nào, thông thường nó bị bỏ qua trong khi định dạng tệp này.

**Tóm tắt**
![](https://i.imgur.com/qz1AWNZ.png)