# Syslog : The Complete System Administrator Guide
## Mục lục

- [Tổng quan về Log](#log)
- [Tổng quan Syslog](#syslog)

[I - Mục đích của Syslog?](#I)

[II - Kiến trúc Syslog?](#II)

[III - Định dạng tin nhắn Syslog?](#III)

- [a - Cấp độ cơ sở Syslog (Syslog facility levels)?](#3a)
   
- [b - Mức độ cảnh báo của Syslog (Syslog severity levels)?](#3b)
   
- [c - PRI?](#3c)
   
- [d - Header?](#3d)

[IV - Syslog gửi tin nhắn hoạt động như thế nào?](#IV)

- [a - Chuyển tiếp nhật ký hệ thống là gì?](#4a)
- [b - Syslog có sử dụng TCP hoặc UDP không?](#4b)

[V - Quá trình phát triển?](#V)


---------------------------
<a name = "log"></a>
## Tổng quan về Log

<img src=https://imgur.com/Y4OBOSr.jpg>

- `Log` ghi lại liên tục các thông báo về hoạt động của cả hệ thống hoặc của các dịch vụ được triển khai trên hệ thống và file tương ứng. Log file thường là các file văn bản thông thường dưới dạng “clear text” tức là bạn có thể dễ dàng đọc được nó, vì thế có thể sử dụng các trình soạn thảo văn bản (vi, vim, nano...) hoặc các trình xem văn bản thông thường (cat, tailf, head...) là có thể xem được file log.
- Các file log có thể nói cho bạn bất cứ thứ gì bạn cần biết, để giải quyết các rắc rối mà bạn gặp phải miễn là bạn biết ứng dụng nào, tiến trình nào được ghi vào log nào cụ thể.
- Trong hầu hết hệ thống Linux thì `/var/log` là nơi lưu lại tất cả các log.

<a name = "syslog"></a>
## Tổng quan Syslog

<img src=https://imgur.com/Eysib9I.png>

- Syslog là một giao thức client/server là giao thức dùng để chuyển log và thông điệp đến máy nhận log. Máy nhận log thường được gọi là syslogd, syslog daemon hoặc syslog server. Syslog có thể gửi qua UDP hoặc TCP. Các dữ liệu được gửi dạng cleartext. Syslog dùng cổng 514.

- Syslog được phát triển năm 1980 bởi Eric Allman, nó là một phần của dự án Sendmail, và ban đầu chỉ được sử dụng duy nhất cho Sendmail. Nó đã thể hiện giá trị của mình và các ứng dụng khác cũng bắt đầu sử dụng nó. Syslog hiện nay trở thành giải pháp khai thác log tiêu chuẩn trên Unix-Linux cũng như trên hàng loạt các hệ điều hành khác và thường được tìm thấy trong các thiết bị mạng như router Trong năm 2009, Internet Engineering Task Forec (IETF) đưa ra chuẩn syslog trong RFC 5424.

- Trong chuẩn syslog, mỗi thông báo đều được dán nhãn và được gán các mức độ nghiêm trọng khác nhau. Các loại phần mềm sau có thể sinh ra thông báo: auth, authPriv, daemon, cron, ftp, dhcp, kern, mail, syslog, user,... Với các mức độ nghiêm trọng từ cao nhất trở xuống Emergency, Alert, Critical, Error, Warning, Notice, Info, and Debug.

<a name = "I"></a>
## I - Mục đích của Syslog?

Syslog được sử dụng như một tiêu chuẩn để sản xuất, chuyển tiếp và thu thập log được sản xuất trên một phiên bản Linux. Syslog xác định mức độ nghiêm trọng (severity levels) cũng như mức độ cơ sở (facility levels) giúp người dùng hiểu rõ hơn về nhật ký được sinh ra trên máy tính của họ. Log (nhật ký) có thể được phân tích và hiển thị trên các máy chủ được gọi là máy chủ Syslog.

Dưới đây là một vài lý do tại sao giao thức **syslog** được thiết kế ở vị trí đầu tiên:

- **Defining an architecture** (xác định kiến ​​trúc) : Syslog là một giao thức, nó là một phần của kiến ​​trúc mạng hoàn chỉnh, với nhiều máy khách và máy chủ. 
- **Message format** (định dạng tin nhắn) : syslog xác định cách định dạng tin nhắn. Điều này rõ ràng cần phải được chuẩn hóa vì các bản ghi thường được phân tích cú pháp và lưu trữ vào các công cụ lưu trữ khác nhau. Do đó, chúng ta cần xác định những gì một máy khách syslog có thể tạo ra và những gì một máy chủ nhật ký hệ thống có thể nhận được.
- **Specifying reliability** (chỉ định độ tin cậy) : syslog cần xác định cách xử lý các tin nhắn không thể gửi được. Là một phần của TCP/IP, syslog rõ ràng sẽ bị thay đổi trên giao thức mạng cơ bản (TCP hoặc UDP) để lựa chọn.
- **Dealing with authentication or message authenticity** (xử lý xác thực hoặc xác thực thư): syslog cần một cách đáng tin cậy để đảm bảo rằng máy khách và máy chủ đang nói chuyện một cách an toàn và tin nhắn nhận được không bị thay đổi.

<a name = "II"></a>
## II - Kiến trúc Syslog?

<img src=https://imgur.com/xIaKX0Y.png>

- Chúng ta có thể nói rằng một máy Linux độc lập hoạt động như một máy chủ máy chủ syslog của riêng mình: nó tạo ra dữ liệu nhật ký, nó được thu thập bởi rsyslog và được lưu trữ ngay vào hệ thống tệp.

Đây là một tập hợp các ví dụ kiến ​​trúc xung quanh nguyên tắc này:

<img src=https://imgur.com/5VjvE4p.png>

<img src=https://imgur.com/G6Lx5UK.png>

<img src=https://imgur.com/BPFsRLZ.png>

<a name = "III"></a>
## III - Định dạng tin nhắn Syslog?

<img src=https://imgur.com/mrgLnFa.png>

Định dạng nhật ký hệ thống được chia thành ba phần, độ dài một thông báo không được vượt quá 1024 bytes::

- **PRI** : chi tiết các mức độ ưu tiên của tin nhắn (từ tin nhắn gỡ lỗi (debug) đến trường hợp khẩn cấp) cũng như các mức độ cơ sở (mail, auth, kernel).
- **Header**: bao gồm hai trường là TIMESTAMP và HOSTNAME, tên máy chủ là tên máy gửi nhật ký.
- **MSG**: phần này chứa thông tin thực tế về sự kiện đã xảy ra. Nó cũng được chia thành trường TAG và trường CONTENT.

<a name = "3a"></a>
### a - Cấp độ cơ sở Syslog (Syslog facility levels)?

- Một mức độ cơ sở được sử dụng để xác định chương trình hoặc một phần của hệ thống tạo ra các bản ghi.

- Theo mặc định, một số phần trong hệ thống của bạn được cung cấp các mức facility như kernel sử dụng `kern facility` hoặc hệ thống mail của bạn bằng cách sử dụng `mail facility`.

- Nếu một bên thứ ba muốn phát hành log, có thể đó sẽ là một tập hợp các cấp độ facility được bảo lưu từ 16 đến 23 được gọi là "local use” facility levels.

- Ngoài ra, họ có thể sử dụng tiện ích của người dùng cấp độ người dùng (“user-level” facility), nghĩa là họ sẽ đưa ra các log liên quan đến người dùng đã ban hành các lệnh.

Dưới đây là các cấp độ facility Syslog được mô tả trong bảng:

Numerical Code|	Keyword	|Facility name
-------|----------|--------
0|	kern|	Những log mà do kernel sinh ra
1|	user|	Log ghi lại cấp độ người dùng
2|	mail|	Log của hệ thống mail
3	|daemon|		Log của các tiến trình nền trên hệ thống
4	|auth|Log từ quá trình đăng nhập hệ hoặc xác thực hệ thống
5|	syslog|	Log từ chương trình syslogd
6|	lpr	|Log từ quá trình in ấn
7	|news|Thông tin tin tức từ hệ thống, liên quan đến giao thức Network News Protocol (nntp)
8	|uucp|	Tập hợp các chương trình cấp thấp, cho phép kết nối các máy Unix với nhau
9	|cron|	Tiện ích cho phép thực hiện các tác vụ theo định kì
10	|authpriv|	Các thống báo liên quan đến truy cập và bảo mật
11	|ftp	|Log của FTP deamon
12	|ntp|	Hệ thống con NTP
13	|security|	Kiểm tra đăng nhập
14	|console|	Log cảnh báo hệ thống
15	|solaris-cron|	Log lịch trình
16-23	|local0 to local7|	Log dự trữ cho sử dụng nội bộ

<a name = "3b"></a>
### b - Mức độ cảnh báo của Syslog?

Mức độ cảnh báo của Syslog được sử dụng để mức độ nghiêm trọng của log event và chúng bao gồm từ gỡ lỗi (debug), thông báo thông tin (informational messages) đến mức khẩn cấp (emergency levels).

Tương tự như cấp độ cơ sở Syslog, mức độ cảnh báo được chia thành các loại số từ 0 đến 7, 0 là cấp độ khẩn cấp quan trọng nhất .

Dưới đây là các mức độ nghiêm trọng của syslog được mô tả trong bảng:

Value|	Severity|	Keyword
---|------|------
0|	Emergency|	`emerg` - Thông báo tình trạng khẩn cấp
1|	Alert	|`alert` - Hệ thống cần can thiệp ngay
2|	Critical|	`crit` - Tình trạng nguy kịch
3|	Error	|`err` - Thông báo lỗi đối với hệ thống
4|	Warning|	`warning` - Mức cảnh báo đối với hệ thống
5|	Notice|	`notice` - Chú ý đối với hệ thống
6|	Informational|	`info` - Thông tin của hệ thống
7|	Debug|	`debug` - Quá trình kiểm tra hệ thống

- Ngay cả khi các bản ghi được lưu trữ theo tên cơ sở theo mặc định, bạn hoàn toàn có thể quyết định lưu trữ chúng theo mức độ nghiêm trọng.

- Nếu bạn đang sử dụng **rsyslog** làm máy chủ syslog mặc định, bạn có thể kiểm tra các thuộc tính **rsyslog** để định cấu hình cách các bản ghi được phân tách.

<a name = "3c"></a>
### c - PRI?

*Đoạn PRI là phần đầu tiên mà bạn sẽ đọc trên một tin nhắn được định dạng syslog.*

Phần `PRI` hay `Priority` là một số được đặt trong ngoặc nhọn, thể hiện cơ sở sinh ra log hoặc mức độ nghiêm trọng, là một số gồm 8 bit:

- 3 bit đầu tiên thể hiện cho tính nghiêm trọng của thông báo.
- 5 bit còn lại đại diện cho sơ sở sinh ra thông báo.

<img src=https://imgur.com/bzc1FbS.png>

Vậy biết một số `Priority` thì làm thế nào để biết nguồn sinh log và mức độ nghiêm trọng của nó. Ta xét 1 ví dụ sau:

`Priority = 191 Lấy 191:8 = 23.875 -> Facility = 23 ("local 7") -> Severity = 191 - (23 * 8 ) = 7 (debug)`

<a name = "3d"></a>
### d - Header?

**Header** bao gồm:
- `TIMESTAMP` : được định dạng trên định dạng của `Mmm dd hh: mm: ss` - `Mmm`, là ba chữ cái đầu tiên của tháng. Thời gian mà thông báo được tạo ra. Thời gian này được lấy từ thời gian hệ thống 
    - *Chú ý: nếu như thời gian của server và thời gian của client khác nhau thì thông báo ghi trên log được gửi lên server là thời gian của máy client*
- `HOSTNAME` (đôi khi có thể được phân giải thành địa chỉ IP). Nó thường được đưa ra khi bạn nhập lệnh tên máy chủ. Nếu không tìm thấy, nó sẽ được gán cả IPv4 hoặc IPv6 của máy chủ.

```
[root@vqmanh log]# hostname
vqmanh.vn
```


<img src=https://imgur.com/PbczEmo.png>

<a name = "IV"></a>
## IV - Syslog gửi tin nhắn hoạt động như thế nào?

Khi phát hành một thông báo nhật ký hệ thống, bạn muốn đảm bảo rằng bạn sử dụng các cách đáng tin cậy và an toàn để cung cấp dữ liệu nhật ký.

<a name = "4a"></a>
### a - Chuyển tiếp nhật ký hệ thống là gì?

- Chuyển tiếp nhật ký hệ thống (syslog forwarding) bao gồm gửi log máy khách đến một máy chủ từ xa để chúng được tập trung hóa, giúp phân tích log dễ dàng hơn.

- Hầu hết thời gian, quản trị viên hệ thống không giám sát một máy duy nhất, nhưng họ phải giám sát hàng chục máy, tại chỗ và ngoài trang web.

- Kết quả là, việc gửi nhật ký đến một máy ở xa, được gọi là máy chủ ghi log tập trung, sử dụng các giao thức truyền thông khác nhau như UDP hoặc TCP.

<a name = "4b"></a>
### b - Syslog có sử dụng TCP hoặc UDP không?

- Syslog ban đầu sử dụng UDP, điều này là không đảm bảo cho việc truyền tin. Tuy nhiên sau đó IETF đã ban hành RFC 3195 (Đảm bảo tin cậy cho syslog) và RFC 6587 (Truyền tải thông báo syslog qua TCP). Điều này có nghĩa là ngoài UDP thì giờ đây syslog cũng đã sử dụng TCP để đảm bảo an toàn cho quá trình truyền tin.

- Syslog sử dụng port 514 cho UDP.

- Tuy nhiên, trên các triển khai log hệ thống gần đây như `rsyslog` hoặc `syslog-ng`, bạn có thể sử dụng TCP làm kênh liên lạc an toàn.

- `Rsyslog` sử dụng port 10514 cho TCP, đảm bảo rằng không có gói tin nào bị mất trên đường đi.

- Bạn có thể sử dụng giao thức TLS/SSL trên TCP để mã hóa các gói Syslog của bạn, đảm bảo rằng không có cuộc tấn công trung gian nào có thể được thực hiện để theo dõi log của bạn.

<a name = "V"></a>
### V - Quá trình phát triển?

- `Syslog daemon` : xuất bản năm 1980, `syslog daemon` có lẽ là triển khai đầu tiên từng được thực hiện và chỉ hỗ trợ một bộ tính năng giới hạn (chẳng hạn như truyền UDP). Nó thường được gọi là `daemon sysklogd` trên Linux;
- `Syslog-ng` : xuất bản năm 1998, `syslog-ng` mở rộng tập hợp các khả năng của trình nền `syslog` gốc bao gồm chuyển tiếp TCP (do đó nâng cao độ tin cậy), mã hóa TLS và bộ lọc dựa trên nội dung. Bạn cũng có thể lưu trữ log vào cơ sở dữ liệu trên local để phân tích thêm.

<img src=https://imgur.com/DAsMGlF.png>

- `Rsyslog` - "The rocket-fast system for log processing" được bắt đầu phát triển từ năm 2004 bởi Rainer Gerhards `rsyslog` là một phần mềm mã nguồn mở sử dụng trên Linux dùng để chuyển tiếp các log message đến một địa chỉ trên mạng (log receiver, log server) Nó thực hiện giao thức syslog cơ bản, đặc biệt là sử dụng TCP cho việc truyền tải log từ client tới server. Hiện nay `rsyslog` là phần mềm được cài đặt sẵn trên hầu hết hệ thống Unix và các bản phân phối của Linux như : Fedora, openSUSE, Debian, Ubuntu, Red Hat Enterprise Linux, FreeBSD…

<img src=https://imgur.com/AHiIOSg.png>

