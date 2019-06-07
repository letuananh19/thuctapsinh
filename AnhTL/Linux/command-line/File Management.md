# File management

### I. Di chuyển / Liệt kê các tập tin

**1. Hiển thị tên thư mục đang làm việc**
``pwd``
**2.  di chuyển qua lại các thư mục**
- Di chuyển tới thư mục home << /home/người_dùng >>

    `` cd ``
    
- Di chuyển ra thư mục mẹ của thư mục hiện tại.

    `` cd .. ``
    
- Hoán đổi vị trí đứng với thư mục này và thư mục mẹ của nó.

    `` cd - ``
    
- Di chuyển tới thư mục << /home/người_dùng/Desktop >>

    `` cd ~ ``
    
    - VD: Di chuyển vào trong thư mục sbin

`` cd /usr/sbin ``

**3. Liệt kê các danh mục trong thư mục hiện hành**
``ls``
- Liệt kê danh mục tập tin trong thư mục 1 cách chi tiết.
`` ls -l ``
- Liệt kê tất cả các tập tin, kể cả các tập tin ẩn (thường có tên bắt đầu bằng một dấu chấm)
`` ls -a ``
- Liệt kê tên các thư mục nằm trong thư mục hiện hành.
`` ls -d ``
- Xếp lại các tập tin theo ngày đã tạo ra, bắt đầu bằng những tập tin mới nhất.
`` ls -t ``
- Xếp lại các tập tin theo kích thước, từ to nhất đến nhỏ nhất.
`` ls -S ``
- Liệt kê theo từng trang một, nhờ tiện ích << more >>
`` ls -l | more ``

### II. Tập tin và thư mục
**1. Lệnh dùng để copy (cp)**
 - sao chép từ file1 sang file2.
 
`` cp file1 file2``

- chép file vào thự mục.

`` cp file thưmục ``

- kiểm tra xem có ghi đè không.

`` cp -i file1 file2``
    
- Sao chép mà không cần xác nhận là có ghi đè hay không.

`` cp -f file1 file2 ``
   
- Nếu đích sao chép có cùng tên tệp, tạo bản sao lưu và sao chép.

`` cp -b file1 file2``
    
- Sao chép tất cả nội dung của ``thưmục1`` sang ``thưmục2``.

`` cp -r thưmục1 thưmục2 ``
    
**2. Chuyển tên tập tin/ thư mục.**
`` mv ``
- chuyển tên file1 thành tên file2.
`` mv file1 file2 ``

- Chuyển tên thưmục1 thành thưmục2.
`` mv thưmục1 thưmục2 ``

- Chuyển file1 vào thư mục1.
`` mv file1 thưmục1 ``

- Chuyển ``file111`` vào ``thưmục1`` rồi đồng thời đổi tên tên tin thành ``file2``.

`` mv file1 thưmục1/file2 ``

**3. xóa bỏ tập tin/ thư mục**
`` rm ``

- Xóa bỏ file trong thư mục hiện hành.

`` rm file ``

- Xóa bỏ thư mục trống mang tên ``thưmục1``.

`` rmdir thưmục1 ``

- Xóa bỏ thư mục mang tên ``thưmục1`` và tất cả các tập tin trong đó. 

`` rm -rf thưmục1 ``

**4. Tạo ta một liên kết mang tên ``abc`` đến file1 (nối tắt).**

`` ln -s file1 abc ``
**5. Tìm tập tin mang tên ``file1`` trong `` thưmục1`` kể cả trong các thư mục con.**

`` find thưmục1 -name file1 ``

**6. So sánh nội dung của 2 tập tin hoặc thư mục.**

`` diff file1 file2``

### III. Nội dung tập tin
**1. Đọc nội dung file**
- Xuất hiện nội dung của ``file1`` trên màn hình ở dạng mã ASCII.

`` cat file1 ``

- Xuất hiện nội dung của tập tin ``file1`` trên màn hình theo chế độ từng trang một: ấn phím << enter >> để xuống dòng; ấn phím << space >> để sang thêm 1 trang; ấn phím << q >> để thoát.

`` more file1 ``

- << less >> giống như << more >>, nhưng cho phép dùng phím << page Down >>.

`` less file1 ``

**2. Đếm số dòng**
- Xuất hiện n số dòng đầu tiên của tập tin ``file1``.

`` head -n file1 ``

- Xuất hiện số n dòng cuối cùng của tập tin ``file1``.

`` tail -n file1 ``

**3. Soạn thảo tập tin.**
- Soạn thảo tập tin ``file1``, dùng trình soạn thảo vi.

`` vi file1 ``

- Soạn thảo tập tin ``file1``, dùng trình soạn thảo nano.

`` nano file1 ``

- Soạn thảo tập tin ``file1``, dùng trình soạn thảo gedit

`` gedit file1 ``

**4. Tìm nội dung**
- Xuất hiện các dòng chứa nội dung ``chuỗi`` trong tập tin ``file1``.

`` grep chuỗi file1 ``

- Tìm tất cả nội dung `` chuỗi `` trong tất cả các tập tin trong thư mục mang tên ``thưmục1``.

`` grep -r chuỗi thưmục1 ``

**5. Ghi thêm nội dung vào file**.
-  Ghi nội dung vào ``file1` ( dùng lệnh này sẽ chèn luôn cả nội dung đã có sẵn trong file ).

`` echo nội_dung > file1``

- Bổ sung nội dung tiếp tục ở phần cuối của tập tin ``file1``.

`` echo nội_dung >> file2 ``

### IV. Quyền truy cập tập tin
**1. Xác định tập tin**
- Xác định người chủ của tập tin ``file1`` là người dùng mang tên <<tên_người_dùng>>.

`` chown tên_người_dùng file1 ``

- Xác định người chủ của thư mực ``thưmục1``, kể cả các thư mục còn (-R) là người dùng <<tên_người_dùng>>.

`` chown -R tên_người_dùng thưmục1``

- Chuyển tập tin ``file1`` thành sở hữu của group người dùng mang tên `` group1 ``.

`` chgrp group1 file1 ``

**2. Phân quyền**
- Giao (+) quyền thực thi (x) tập tin ``file1`` cho người dùng (u).

`` chmod u+x file1 ``

- Rút (-) quyền ghi (w) ``file1`` của ``group1`` (g).

`` chmod g-w file1``

- Rút (-) quyền đọc (r) tập tin ``file1`` của những người dùng khác (o).

`` chmod o-r file1``

- Giao (+) quyền đọc (r) và ghi (w) của ``file1`` cho mọi người (a).

`` chmod a+rw file1 ``

- Giao (+) quyền đọc (r) và quyền thực thi (x) vào bên trong ``thưmục1``, kể cả tất cả các thư mục con của nó (-R), cho tất cả mọi người (a).

`` chmod -R a+rw thưmục1 ``





