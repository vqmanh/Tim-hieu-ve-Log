# Ghi log tập trung với Syslog trên Linux

<img src=https://imgur.com/DqcfgnU.jpg>

## Lời mở đầu

- Nếu bạn là quản trị viên hệ thống Linux , có lẽ bạn dành nhiều thời gian để duyệt các tệp nhật ký của mình để tìm thông tin liên quan về các sự kiện trong quá khứ.

- Hầu hết thời gian, bạn không làm việc với một máy đơn lẻ, nhưng với nhiều máy Linux khác nhau , mỗi máy có bộ lưu trữ nhật ký cục bộ riêng.

- Bây giờ nếu bạn đã duyệt nhật ký cho nhiều máy khác nhau, bạn sẽ phải kết nối riêng với từng máy, xác định vị trí nhật ký và cố gắng tìm thông tin mà bạn đang tìm kiếm.

- Điều này là tất nhiên trong trường hợp bạn có thể truy cập vật lý vào máy, giả định rằng máy đã hoạt động và bạn không bị từ chối quyền truy cập vào máy.

- Điều gì xảy ra nếu chúng ta có thể truy cập các tệp nhật ký từ một điểm duy nhất?

## Mục lục	

[I - Logging hoạt động như thê nào?](#1)
- [a - Khái niệm chung về logging](#1a)
- [b - Log được lưu ở đâu?](#1b)

[II - Tổng quan về Log tập trung](#2)

[III - Cấu hình rsyslog để chuyển tiếp log đến máy chủ tập trung](3)
- [a - Cấu hình rsyslog server](#3a)
- [b - Cấu hình rsyslog client](#3b)
- [c - Cấu hình đẩy log Apache về Ryslog Server](#3c)

----------------------------
<a name = "1"></a>
## I - Logging hoạt động như thê nào?
<a name = "1a"></a>
### a - Khái niệm chung về logging

- Linux sử dụng giao thức `syslog` xác định một tiêu chuẩn cho mọi khía cạnh khi đăng nhập vào hệ điều hành (không chỉ Linux mà cả Windows): xác định thông điệp trông như thế nào, mô tả (`severity levels`) mức độ nghiêm trọng của tin nhắn, cũng như liệt kê các cổng mà `syslog` sẽ đang sử dụng.

- `Syslog` có thể được sử dụng như một máy chủ (lưu trữ các bản ghi) hoặc như một máy khách (chuyển tiếp các bản ghi đến một máy chủ từ xa).

- Do đó, giao thức `syslog` cũng xác định cách truyền nhật ký nên được thực hiện, cả theo cách đáng tin cậy và an toàn.

- Nếu bạn đang sử dụng một bản phân phối Linux hiện đại (như máy Ubuntu, CentOS hoặc RHEL), máy chủ `syslog` mặc định được sử dụng là `rsyslog`.

<img src=https://imgur.com/AHiIOSg.png>

- `Rsyslog` là một sự phát triển của `syslog`, cung cấp các khả năng như các mô đun có thể cấu hình, được liên kết với nhiều mục tiêu khác nhau (ví dụ chuyển tiếp nhật ký Apache đến một máy chủ từ xa).

- `Rsyslog` cũng cung cấp tính năng lọc riêng cũng như tạo khuôn mẫu để định dạng dữ liệu sang định dạng tùy chỉnh.
<a name = "1b"></a>
### b - Log được lưu ở đâu?

- Log được lưu trữ tại `/var/log` trên hệ thống.
- `/var/log` không chỉ chứa các tệp mà còn chứa các thư mục chuyên dụng mà nhà cung cấp tạo khi ứng dụng được cài đặt.

**Đây là cấu trúc thư mục ở phía máy chủ.**

<img src= https://imgur.com/qbyGJ23.jpg>
<a name = "2"></a>
## II - Tổng quan về Log tập trung

**Tác dụng của log là vô cùng to lớn vậy làm thế nào để quản lý log tốt hơn?**

- Để quản lý log một cách tốt hơn, xu thế hiện nay sẽ sử dụng log tập trung. Vậy log tập trung là gì? Tác dụng của nó thế nào?

- Hiểu một cách đơn giản : Log tâp trung là quá trình tập trung, thu thập, phân tích... các log cần thiết từ nhiều nguồn khác nhau về một nơi an toàn để thuận lợi cho việc phân tích, theo dõi hệ thống.

<img src=https://imgur.com/ym0t82c.jpg>

**Tại sao lại phải sử dụng log tập trung?**

- Do có nhiều nguồn sinh log

    - Có nhiều nguồn sinh ra log, log nằm trên nhiều máy chủ khác nhau nên khó quản lý.
    - Nội dung log không đồng nhất (Giả sử log từ nguồn 1 có có ghi thông tin về ip mà không ghi thông tin về user name đăng nhập mà log từ nguồn 2 lại có) -> khó khăn trong việc kết hợp các log với nhau để xử lý vấn đề gặp phải.
    - Định dạng log cũng không đồng nhất -> khó khăn trong việc chuẩn hóa
- Đảm bảo tính toàn vẹn, bí mật, sẵn sàng của log.

    - Do có nhiều các rootkit được thiết kế để xóa bỏ logs.
    - Do log mới được ghi đè lên log cũ -> Log phải được lưu trữ ở một nơi an toàn và phải có kênh truyền đủ đảm bảo tính an toàn và sẵn sàng sử dụng để phân tích hệ thống.

**Ưu điểm:**

- Giúp quản trị viên có cái nhìn chi tiết về hệ thống -> có định hướng tốt hơn về hướng giải quyết
- Mọi hoạt động của hệ thống được ghi lại và lưu trữ ở một nơi an toàn (log server) -> đảm bảo tính toàn vẹn phục vụ cho quá trình phân tích điều tra các cuộc tấn công vào hệ thống
- Log tập trung kết hợp với các ứng dụng thu thập và phân tích log khác nữa giúp cho việc phân tích log trở nên thuận lợi hơn -> giảm thiểu nguồn nhân lực.

**Nhược điểm:**

- Bạn có nguy cơ quá tải máy chủ `syslog` của mình: với cấu ​​trúc này, bạn đang đẩy các bản ghi đến một máy chủ từ xa. Hậu quả là, nếu một máy bị tấn công và bắt đầu gửi hàng ngàn log messages, có nguy cơ làm quá tải máy chủ log.
- Nếu máy chủ nhật ký của bạn bị hỏng, bạn sẽ mất khả năng xem tất cả các nhật ký được gửi bởi khách hàng. Hơn nữa, nếu máy chủ ngừng hoạt động, máy khách sẽ bắt đầu lưu trữ thư cục bộ cho đến khi máy chủ khả dụng trở lại, do đó không gian đĩa ở phía máy khách sẽ dần bị đầy.


<a name = "3"></a>
## III - Cấu hình `rsyslog` để chuyển tiếp log đến máy chủ tập trung

**Mặc định file cấu hình `rsyslog` sẽ được lưu trên thư mục `/etc/` bao gồm file `rsyslog.conf` và thư mục `rsyslog.d`**
<img src=https://imgur.com/3e8KL7r.jpg>

### Mô hình:

<img src=https://imgur.com/bvcvrel.jpg>

### IP Planning

Tên máy ảo|	Hệ điều hành|	IP address|	Subnet mask|	Default gateway
------|-----|----------|--------|-----
Rsyslog Server| CentOS7|66.0.0.199|/24|66.0.0.1
Rsyslog Client1|CentOS7|66.0.0.200|/24|66.0.0.1
Rsyslog Client2|Ubuntu19.04|66.0.0.113|/24|66.0.0.1
### Triển khai
<a name = "3a"></a>
### a - Cấu hình Rsyslog Server
**Bước 1: Chỉnh sửa trong file cấu hình /etc/rsyslog.conf của máy chủ Syslog-server để nó có thể nhận các bản tin log từ các client gửi về.**

- Bỏ comment 2 dòng sau: bạn có thể lựa chọn sử dụng UDP hoặc TCP, ở đây mình bỏ comment dòng UDP

<img src=https://imgur.com/5aab9K2.jpg>

- Để máy chủ log tạo thành các thư mục lưu riêng log đối với từng máy Client gửi về thêm dòng này vào cuối file cấu hình.

```
$template RemoteServer, "/var/log/%HOSTNAME%/%SYSLOGFACILITY-TEXT%.log"
*.* ?RemoteServer
```
<img src=https://imgur.com/pSeTixx.jpg>

*Với cú pháp này, các tệp cập nhật sẽ được nhóm theo tên máy client gửi `log` và sau đó theo cơ sở `syslog` (kern, user, auth, v.v.)*

HOẶC
```
$template TmplAuth,"/var/log/%HOSTNAME%/%PROGRAMNAME%.log"
*.*     ?TmplAuth
```
- Ngoài ra bạn có thể sửa `%HOSTNAME%` thay bằng `%fromhost-ip%`
để thư mục trả về sẽ là Ip-server client.

```
$template TmplAuth,"/var/log/%fromhost-ip%/%SYSLOGFACILITY-TEXT%.log"
*.*     ?TmplAuth
```


**Bước 2: Khởi động lại máy chủ rsyslog của bạn và đảm bảo rằng nó hiện đang lắng nghe trên cổng 514 cho UDP hoặc TCP**

```
[root@vqmanh ~]# systemctl restart rsyslog
[root@vqmanh ~]# netstat -una | grep :514
udp        0      0 0.0.0.0:514             0.0.0.0:*
udp6       0      0 :::514                  :::*
# Nếu bạn sử dụng TCP có thể sử dụng lệnh:  
netstat -tna | grep :514
```
**Mở port 514**

***Đối với Firewalld*** 
```
firewall-cmd --permanent --add-port=514/udp
firewall-cmd --permanent --add-port=514/tcp
firewall-cmd --reload
```

<a name = "3b"></a>
### b - Cấu hình Rsyslog Client

**Trên client cũng phải truyền đúng với giao thức như trên server**
- `*.*  @IPserver:514` : Đối với giao thức UDP

- `*.*  @@IPserver:514` : Đối với giao thức TCP

### **Đối với client CentOS7**

**Khai báo IP của Syslog server (dưới mục Rule):**
```
[root@syslog ~]# vi /etc/rsyslog.conf
*.*             @66.0.0.199:514
```
<img src=https://imgur.com/nh8NWvw.jpg>

**Restart service Rsyslog**

`systemctl restart rsyslog`

### Đối với client Ubuntu

**Khai báo IP của Syslog server trên file mặc định của Rsyslog**

```
root@ubuntu:/etc# vi /etc/rsyslog.d/50-default.conf 
*.*             @66.0.0.199:514
```



- VD: @66.0.0.199:514 - UDP

     @@66.0.0.199:514 - TCP


<img src=https://imgur.com/0dSKy55.jpg>

**Restart service Rsyslog**

`systemctl restart rsyslog`

### Chuyển qua Rsyslog Server

**Kiểm tra trên thư mục /var/log sẽ xuất hiện 2 thư mục có tên là tên của 2 máy client.**

<img src=https://imgur.com/LfQ3JpI.jpg>

Các bạn có thể thấy xuất hiện thêm 2 thư mục là `/syslog` tên máy client centos và `/ubuntu` tên máy client ubuntu của mình

**Kiểm tra trên thư mục client:**

<img src=https://imgur.com/XKU7XAi.jpg>

Các tệp tin log được đặt theo `SYSLOGFACILITY`
### Đứng trên Server sử dụng tcpdump bắt gói tin của client gửi sang trên port 514

<img src =https://imgur.com/geVbJzL.jpg>

Khi bật dịch vụ Rsyslog lên, các gói tin từ client sẽ đẩy về Rsyslog Server qua port 514 theo giao thức UDP, vì mình cấu hình theo giao thức này.

### **Đứng trên client bắt gói tin từ client đẩy về server**

VD: Trên Client Ubuntu


<img src=https://imgur.com/nXXGKOr.jpg>

VD: Tương tự trên Client CentOS7

<img src=https://imgur.com/EbR0IGM.jpg>

Đây là log được đẩy về khi mình `ssh` vào Client CentOS7

- ***Mặc định port của client sẽ lớn hơn 1024 và sẽ luôn thay đổi.***

- ***Như bạn thấy khi cấu hình Rsyslog, các log trên client sẽ tự động đẩy về Rsyslog Server.***

<a name = "3c"></a>
## Cấu hình đẩy log Apache về Ryslog Server

### Trên Client CentOS đã cài Apache


**Cách 1:**

**Bước 1: Cấu hình file httpd.conf**

`vi /etc/httpd/conf/httpd.conf`

**Thêm dòng sau vào cuối file cấu hình**

```
CustomLog "| /bin/sh -c '/usr/bin/tee -a /var/log/httpd/httpd-access.log | /usr/bin/logger -thttpd -plocal1.notice'" combined
ErrorLog "|/bin/sh -c '/usr/bin/tee -a /var/log/httpd/httpd-error.log | /usr/bin/logger -thttpd -plocal1.err'"
```
<img src=https://imgur.com/gf1x9dk.jpg>


**Cách 2:**

Tạo và thêm cấu hính sau sau vào file:

`vi /etc/rsyslog.d/apache.conf`

```
$ModLoad imfile #Dòng này chỉ thêm một lần

# Default Apache Error Log 
$InputFileName /var/log/httpd/error_log #Đường dẫn file log muốn đẩy
$InputFileTag errorlog #Tên file 
$InputFileSeverity info #Các log từ mức info trở lên được ghi lại
$InputFileFacility local3 #Facility log
$InputRunFileMonitor

# Default Apache Access Log
$InputFileName /var/log/httpd/access_log
$InputFileTag accesslog
$InputFileSeverity info
$InputFileFacility local4
$InputRunFileMonitor

$InputFilePollInterval 10 #Cứ sau 10 giây lại gửi tin nhắn

```

**Bước 2: Restart dịch vụ**

`systemctl restart httpd`

`systemctl restart rsyslog`

### Kiểm tra trên Rsyslog Server

**Chú ý: Với 2 định dạng cấu hình file log đẩy về server**
 - Định dạng 1: `%PROGRAMNAME%.log` - tên theo các chương trình
    - Với định dạng này log apache được đẩy về server theo Cách 1 ở trên: file chứa log apache có tên `httpd.log`
    - Theo cách 2 tên file log sẽ là: `errorlog.log` và `accesslog.log` như cấu hình ở trên
- Định dạng 2: `%SYSLOGFACILITY-TEXT%.log` tên theo cơ sở sinh ra log
    - Log apache được đẩy về theo cách 1: tên file chứa log sẽ là `local1.log` chứa cả `errorlog` lẫn `accesslog`
    - Theo cách 2, tên file chứa log sẽ là `local3.log` ứng với `errorlog`, `local4.log` ứng với `accesslog`


### Cấu hình lọc Accesslog trước khi gửi

**VD: Nếu bạn muốn loại bỏ tất cả mã thông báo trong đó mã trạng thái `http` không phải 404 hoặc 502.** 

Thêm cấu hình sau vào file apache.conf tạo ở trên:
- Định dạng 1: `%PROGRAMNAME%.log`

`if $programname == 'accesslog' and not ($msg contains '404' or $msg contains '502') then stop`

- Định dạng 2: `%SYSLOGFACILITY-TEXT%.log`

`if $SYSLOGFACILITY-TEXT == 'local4' and not ($msg contains '404' or $msg contains '502') then stop`

```
HTTP Status Messages

Các HTTP Status Messages hay còn được biết đển là các http code có 5 loại chính và ý nghĩa các mã cơ bản đó là:
1xx: Thông tin (yêu cầu (request) đã được nhận, tiếp tục tiến trình xử lí)
100 Continue: Server đã nhận được yêu cầu header
101 Switching Protocols
103 Checkpoint
2xx: Thành công
200 OK: yêu cầu gửi lên server đã được tiếp nhận thành công
201 Created
202 Accepted
203 Non-Authoritative Information
204 No Content
205 Reset Content
206 Partial Content
3xx: Điều hướng
300 Multiple Choices
301 Moved Permanently: Trang web yêu cầu đã được chuyển đến một địa chỉ URL mới
302 Found : Trang web yêu cầu đã được chuyển tạm thời đến một địa chỉ URL mới
303 See Other
304 Not Modified
306 Switch Proxy
307 Temporary Redirect
308 Resume Incomplete
4xx: Lỗi Client
400 Bad Request: Lỗi cú pháp, yêu cầu không thể thực hiện được
401 Unauthorized
402 Payment Required
403 Forbidden: Không được phép truy cập vào đây
404 Not Found: Không tìm thấy trang địa chỉ với URL hiện tại
5xx: Lỗi Server
500 Internal Server Error: Một thông báo lỗi chung, được đưa ra khi không có thông báo cụ thể nào khác phù hợp
501 Not Implemented: Máy chủ hoặc không nhận ra phương thức yêu cầu, hoặc nó không có khả năng thực hiện yêu cầu
502 Bad Gateway: Máy chủ đã hoạt động như một cổng hoặc proxy và nhận được phản hồi không hợp lệ từ máy chủ
503 Service Unavailable: Máy chủ hiện không có (quá tải)
504 Gateway Timeout: Máy chủ hoạt động như một gateway hoặc proxy và không nhận được phản hồi kịp thời từ máy chủ phía trên
```