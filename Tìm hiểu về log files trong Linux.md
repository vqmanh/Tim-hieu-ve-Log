# Tìm hiểu về log files trong Linux

- *Như một tiêu chuẩn chung trong hầu hết các hệ thống Linux, các tập tin log được đặt trong thư mục `/var/log`. Bất kỳ ứng dụng khác mà sau này bạn có thể cài đặt trên hệ thống của bạn có thể sẽ tạo tập tin log của chúng tại `/var/log`. Dùng lệnh `ls -l /var/log` để xem nội dung của thư mục này.*


## **Sau đây là 20 tệp log khác nhau được đặt trong thư mục `/var/log/`.**

- `/var/log/message` - Chứa các thông báo hệ thống toàn cầu, bao gồm các tin nhắn được ghi lại trong quá trình khởi động hệ thống. Có một số thứ được đăng nhập / var / log / message bao gồm mail, cron, daemon, kern, auth, v.v.
- `/var/log/dmesg` - Chứa thông tin bộ đệm vòng kernel. Khi hệ thống khởi động, nó sẽ in số lượng tin nhắn trên màn hình hiển thị thông tin về các thiết bị phần cứng mà hạt nhân phát hiện được trong quá trình khởi động. Các tin nhắn này có sẵn trong bộ đệm vòng kernel và bất cứ khi nào tin nhắn mới xuất hiện, tin nhắn cũ sẽ bị ghi đè. Bạn cũng có thể xem nội dung của tệp này bằng lệnh `dmesg` .
- `/var/log/auth.log` - Chứa thông tin ủy quyền hệ thống, bao gồm thông tin đăng nhập người dùng và máy móc xác thực đã được sử dụng.
- `/var/log/boot.log` - Chứa thông tin được ghi lại khi hệ thống khởi động
- `/var/log/daemon.log` - Chứa thông tin được ghi lại bởi các trình nền khác nhau chạy trên hệ thống
- `/var/log/dpkg.log` - Chứa thông tin được ghi lại khi gói được cài đặt hoặc gỡ bỏ bằng lệnh dpkg
- `/var/log/kern.log` - Chứa thông tin được ghi bởi kernel. Hữu ích cho bạn để khắc phục sự cố một kernel được xây dựng tùy chỉnh.
- `/var/log/lastlog` - Hiển thị thông tin đăng nhập gần đây cho tất cả người dùng. Đây không phải là một tập tin ascii. Bạn nên sử dụng lệnh Lastlog để xem nội dung của tệp này.
- `/var/log/maillog /var/log/mail.log` - Chứa thông tin nhật ký từ máy chủ thư đang chạy trên hệ thống. Ví dụ: sendmail ghi thông tin về tất cả các mục đã gửi vào tệp này
- `/var/log/user.log` - Chứa thông tin về tất cả nhật ký cấp độ người dùng
- `/var/log/Xorg.x.log` - Đăng nhập tin nhắn từ X
- `/var/log/alternatives.log` - Thông tin của các lựa chọn thay thế cập nhật được đăng nhập vào tệp nhật ký này. Trên Ubuntu, các lựa chọn thay thế cập nhật duy trì các liên kết tượng trưng xác định các lệnh mặc định.
- `/var/log/btmp` - Tập tin này chứa thông tin về các thông tin đăng nhập thất bại. Sử dụng lệnh cuối cùng để xem tệp `btmp`. Ví dụ, “last -f /var/log/btmp | more”
- `/var/log/cups` - Tất cả máy in và in các thông điệp nhật ký liên quan
- `/var/log/anaconda.log` - Khi bạn cài đặt Linux, tất cả các thông báo liên quan đến cài đặt được lưu trữ trong tệp nhật ký này
- `/var/log/yum.log` - Chứa thông tin được ghi lại khi gói được cài đặt bằng yum
- `/var/log/cron` - Bất cứ khi nào cron daemon (hoặc anacron ) bắt đầu một công việc cron, nó sẽ ghi lại thông tin về công việc cron trong tệp này
- `/var/log/secure` - Chứa thông tin liên quan đến xác thực và đặc quyền ủy quyền. Ví dụ: sshd ghi lại tất cả các tin nhắn ở đây, bao gồm cả đăng nhập không thành công.
- `/var/log/wtmp` hoặc `/var/log/utmp` - Chứa hồ sơ đăng nhập. Sử dụng wtmp bạn có thể tìm ra ai đã đăng nhập vào hệ thống. lệnh ai sử dụng tập tin này để hiển thị thông tin.
- `/var/log/faillog` - Chứa người dùng đăng nhập thất bại. Sử dụng lệnh faillog để hiển thị nội dung của tệp này.

### **Ngoài các tệp nhật ký trên, thư mục `/var/log` cũng có thể chứa các thư mục con sau tùy thuộc vào ứng dụng đang chạy trên hệ thống của bạn.**

- `/var/log/httpd/` (hoặc) `/var/log/apache2` - Chứa máy chủ web apache `access_log` và `error_log`
- `/var/log/lighttpd/` - Chứa HTTPD access_log và error_log
- `/var/log/conman/` - Các tệp nhật ký cho máy khách ConMan. conman kết nối các bảng điều khiển từ xa được quản lý bởi conmand daemon.
- `/var/log/mail/` - Thư mục con này chứa các nhật ký bổ sung từ máy chủ thư của bạn. Ví dụ: sendmail lưu trữ số liệu thống kê thư được thu thập trong tệp / var / log / mail / stats
- /var/log/prelink/ - chương trình `prelink` sửa đổi các thư viện chia sẻ và nhị phân được liên kết để tăng tốc quá trình khởi động. `/var/log/prelink/prelink.log` chứa thông tin về tệp .so đã được sửa đổi bởi `prelink`.
- `/var/log/audit/` - Chứa thông tin nhật ký được lưu trữ bởi trình nền kiểm toán Linux (audd).
- `/var/log/setroubleshoot/` - SELinux sử dụng setroubledhootd (SE Trouble Shoot Daemon) để thông báo về các vấn đề trong ngữ cảnh bảo mật của tệp và ghi nhật ký thông tin đó vào tệp nhật ký này.
- `/var/log/samba/` - Chứa thông tin nhật ký được lưu trữ bởi samba, được sử dụng để kết nối Windows với Linux.
- `/var/log/sa/` - Chứa các tệp sar hàng ngày được thu thập bởi gói `sysstat` .
- `/var/log/sssd/` - Được sử dụng bởi trình nền dịch vụ bảo mật hệ thống quản lý quyền truy cập vào các thư mục và cơ chế xác thực từ xa.

### Xem và kiểm soát các tập tin log

- **Để kiểm soát các tập tin log, ta sử dụng tiến trình `rsyslogd` và cấu hình của nó nằm `/etc/rsyslog.conf`.**
- Đối với tất cả các tập tin log plain-text, các log có thể được xem bằng lệnh `cat`. Tuy nhiên, nếu các tập tin log là rất lớn, thì bạn có thể sử dụng lệnh `tail` để chỉ hiển thị các phần cuối của tập tin log.
    - `tail -n 500 /var/log/messages` - để xem 500 mục cuối cùng của tập tin.
- Để theo dõi các log trong thời gian thực `tail -f` cũng là một lệnh rất hữu ích mà sẽ giúp theo dõi các thông báo khi có thông tin log gửi đến. Điều này đặc biệt hữu ích khi xử lý sự lưu chuyển mail và lỗi chuyển phát thư.
    - `tail -f /var/log/maillog`

- Một số log Linux như các tập tin nhị phân mà cần phải được phân tích bởi một ứng dụng được thiết kế đặc biệt. Những bản ghi này được lưu trong `/var/log/wtmp` `/var/log/btmp` và `/var/run/utmp`.
    - Để xem nội dung của thư mục /var/log/wtmp dùng: `last`
    - Để xem nội dung của thư mục /var/log/btmp dùng: `lastb`
    - Để xem nội dung của thư mục /var/run/utmp dùng: `who`

#### Tập tin log cụ thể trên Cpanel 
**Apache log files:**
- `/usr/local/apache/logs/` – General Apache logs.
- `/usr/local/apache/domlogs/` – Domain specific logs.

**Exim log files:**
- `/var/log/exim_mainlog`
- `/var/log/exim_rejectlog`

**cPanel log files:**
- `/usr/local/cpanel/logs/` – Tất cả các tin nhắn liên quan cPanel nằm ở thư mục này

#### Tập tin log cụ thể trên DirectAdmin
**DirectAdmin log files**
- `/var/log/directadmin/` – DirectAdmin related logs.

**Apache log files**
- `/var/log/httpd/` – Các Apache web server sẽ tạo log vào thư mục mặc định.
- `/var/log/httpd/domains/` – Đối với tất cả các domain khác tạo log trong sub-directory này.

**FTP log files**
- `/var/log/proftpd/` – Nếu ProFTPD được sử dụng.
- `/var/log/pureftpd.log` – Nếu PureFTPd được sử dụng.

**Exim log files**
- `/var/log/exim/` – Log của exim sẽ được lưu ở đây.

**MySQL log files**
- `/var/lib/mysql/`


#### Tập tin log cụ thể trên CentOS
- `/var/log/yum.log` – Lưu giữ log của Yum package manager.
- `/var/log/httpd` – Trên các hệ thống dựa RedHat/CentOS đây là nơi mà các máy chủ web Apache sẽ lưu trữ các log theo mặc định.

#### Tập tin log cụ thể trên Ubuntu
- `/var/log/apache2/` – Trên hệ thống Ubuntu các bản ghi máy chủ web Apache được lưu trữ trong thư mục này.
- `/var/log/apt/` – Lưu giữ log của Ubuntu’s package management.