# Một số file log thường sử dụng


## log SSH:
```
[root@vqmanh ~]# tailf /var/log/secure | grep ssh 
Login thành công
Sep 17 08:04:28 vqmanh sshd[10703]: pam_unix(sshd:session): session opened for user vqmanh by (uid=0)
Sep 17 08:04:29 vqmanh sshd[10709]: reverse mapping checking getaddrinfo for 254-0-0-66.deltacom.net [66.0.0.254] failed - POSSIBLE BREAK-IN ATTEMPT!
Sep 17 08:04:29 vqmanh sshd[10709]: Accepted password for vqmanh from 66.0.0.254 port 58710 ssh2
Sep 17 08:04:29 vqmanh sshd[10709]: pam_unix(sshd:session): session opened for user vqmanh by (uid=0)
---------------
Login thất bại
Sep 17 08:33:19 vqmanh unix_chkpwd[11264]: password check failed for user (pak)
Sep 17 08:33:19 vqmanh sshd[11262]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=66.0.0.254  user=pak
Sep 17 08:33:21 vqmanh sshd[11262]: Failed password for pak from 66.0.0.254 port 58954 ssh2
-----------------------
Login sai user
Sep 17 10:40:37 vqm sshd[10668]: Invalid user vqmanh from 66.0.0.254 port 60347
Sep 17 10:40:37 vqm sshd[10668]: input_userauth_request: invalid user vqmanh [preauth]
Sep 17 10:40:37 vqm sshd[10668]: pam_unix(sshd:auth): check pass; user unknown
Sep 17 10:40:37 vqm sshd[10668]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=66.0.0.254
Sep 17 10:40:39 vqm sshd[10668]: Failed password for invalid user vqmanh from 66.0.0.254 port 60347 ssh2
--------------


```
Note:
 - **pam_unix - Mô-đun để xác thực mật khẩu truyền thống**

- **"POSSIBLE BREAK-IN ATTEMPT" nghĩa là gì?"**

     - Điều này có nghĩa là chủ sở hữu netblock đã không cập nhật bản ghi PTR cho IP tĩnh trong phạm vi của họ và cho biết bản ghi PTR đã lỗi thời, HOẶC ISP không thiết lập bản ghi ngược thích hợp cho khách hàng IP động của mình. Điều này rất phổ biến, ngay cả đối với các ISP lớn.

    - Cuối cùng, bạn nhận được thông điệp trong nhật ký của mình vì ai đó đến từ IP có hồ sơ PTR không đúng (do một trong những lý do ở trên) đang cố gắng sử dụng tên người dùng phổ biến để thử SSH vào máy chủ của bạn (có thể là tấn công bruteforce hoặc có thể là một lỗi trung thực ).

## Log in: 
```
tailf /var/log/messages

Sep 17 08:19:10 vqmanh systemd: Started Session 8 of user vqmanh.
Sep 17 08:19:10 vqmanh systemd-logind: New session 8 of user vqmanh.
```
## Log out

```
[root@vqm ~]# tailf /var/log/secure |grep ssh
Sep 17 11:11:25 vqm sshd[10699]: pam_unix(sshd:session): session closed for user root
```


## Network
```
tailf /var/log/messages

Sep 17 08:25:40 vqmanh systemd: Stopped LSB: Bring up/down networking.
Sep 17 08:25:40 vqmanh systemd: Starting LSB: Bring up/down networking...
```

## Hồ sơ đăng nhập thành công.
```
[root@vqmanh ~]# last -f /var/log/wtmp
hoặc utmpdump /var/log/wtmp

root     pts/3        66.0.0.254       Tue Sep 17 08:24   still logged in
vqmanh   pts/1        66.0.0.254       Tue Sep 17 08:19   still logged in
root     pts/2        66.0.0.254       Tue Sep 17 08:13   still logged in
vqmanh   pts/1        66.0.0.254       Tue Sep 17 08:04 - 08:18  (00:14)
root     pts/0        66.0.0.254       Tue Sep 17 08:03   still logged in
reboot   system boot  3.10.0-957.27.2. Tue Sep 17 08:01 - 08:28  (00:27)
root     pts/1        66.0.0.254       Mon Sep 16 23:29 - crash  (08:31)
root     pts/0        66.0.0.254       Tue Aug 27 14:41 - crash (20+17:19)
root     tty1                          Tue Aug 27 14:40 - crash (20+17:21)
reboot   system boot  3.10.0-693.el7.x Tue Aug 27 14:39 - 08:28 (20+17:49)
```
## Đăng nhập thất bại
```
[root@vqmanh ~]# lastb -f /var/log/btmp | more
pak      ssh:notty    66.0.0.254       Tue Sep 17 08:33 - 08:33  (00:00)
```
## Log tạo User

```
/var/log/messages
Sep 17 08:32:49 vqmanh useradd[11253]: new group: name=pak, GID=1001
Sep 17 08:32:49 vqmanh useradd[11253]: new user: name=pak, UID=1001, GID=1001, home=/home/pak, shell=/bin/bash
```

## Log access Apache
```
[root@vqmanh httpd]# tailf /var/log/httpd/access_log
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET / HTTP/1.1" 403 4897 "-" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /images/apache_pb.gif HTTP/1.1" 200 2326 "http://66.0.0.200/" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /noindex/css/bootstrap.min.css HTTP/1.1" 200 19341 "http://66.0.0.200/" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /noindex/css/open-sans.css HTTP/1.1" 200 5081 "http://66.0.0.200/" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /images/poweredby.png HTTP/1.1" 200 3956 "http://66.0.0.200/" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /noindex/css/fonts/Bold/OpenSans-Bold.woff HTTP/1.1" 404 239 "http://66.0.0.200/noindex/css/open-sans.css" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /noindex/css/fonts/Light/OpenSans-Light.woff HTTP/1.1" 404 241 "http://66.0.0.200/noindex/css/open-sans.css" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /noindex/css/fonts/Bold/OpenSans-Bold.ttf HTTP/1.1" 404 238 "http://66.0.0.200/noindex/css/open-sans.css" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /noindex/css/fonts/Light/OpenSans-Light.ttf HTTP/1.1" 404 240 "http://66.0.0.200/noindex/css/open-sans.css" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
66.0.0.254 - - [17/Sep/2019:09:14:25 +0700] "GET /favicon.ico HTTP/1.1" 404 209 "http://66.0.0.200/" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) coc_coc_browser/80.0.182 Chrome/74.0.3729.182 Safari/537.36"
::1 - - [17/Sep/2019:09:14:32 +0700] "OPTIONS * HTTP/1.0" 200 - "-" "Apache/2.4.6 (CentOS) (internal dummy connection)"
::1 - - [17/Sep/2019:09:14:33 +0700] "OPTIONS * HTTP/1.0" 200 - "-" "Apache/2.4.6 (CentOS) (internal dummy connection)"
```

## Log error Apache

```
[root@vqmanh httpd]# tailf error_log
[Tue Sep 17 09:13:20.343905 2019] [core:notice] [pid 11438] SELinux policy enabled; httpd running as context system_u:system_r:httpd_t:s0
[Tue Sep 17 09:13:20.348138 2019] [suexec:notice] [pid 11438] AH01232: suEXEC mechanism enabled (wrapper: /usr/sbin/suexec)
[Tue Sep 17 09:13:20.724596 2019] [lbmethod_heartbeat:notice] [pid 11438] AH02282: No slotmem from mod_heartmonitor
[Tue Sep 17 09:13:20.735146 2019] [mpm_prefork:notice] [pid 11438] AH00163: Apache/2.4.6 (CentOS) configured -- resuming normal operations
[Tue Sep 17 09:13:20.735229 2019] [core:notice] [pid 11438] AH00094: Command line: '/usr/sbin/httpd -D FOREGROUND'
[Tue Sep 17 09:14:25.250850 2019] [autoindex:error] [pid 11439] [client 66.0.0.254:59436] AH01276: Cannot serve directory /var/www/html/: No matching DirectoryIndex (index.html) found, and server-generated directory index forbidden by Options directive
```