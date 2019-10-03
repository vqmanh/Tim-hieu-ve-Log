# Tạo file log ghi tất cả các command của người dùng trên Linux

## Bước 1: Sửa file
#### Đối với Ubuntu

`vi /etc/bash.bashrc`

#### Đối với CentOS

`[root@vqmanh log]# vi /etc/bashrc`

Thêm dòng này vào cuối file
```
export PROMPT_COMMAND='RETRN_VAL=$?;logger -p local6.debug "$(whoami) [$$]: $(history 1 | sed "s/^[ ]*[0-9]\+[ ]*//" ) [$RETRN_VAL]"'
```

## Bước 2: Tạo tập tin

`[root@vqmanh log]# vi  /etc/rsyslog.d/bash.conf`

**Thêm dòng sau vào tệp vừa tạo**

`local6.*    /var/log/commands.log`

## Bước 3: Khởi động lại rsyslog

`systemctl restart rsyslog`

Sau đó, `reboot` lại hệ thống, bạn sẽ thấy xuất hiện file `/var/log/commands.log`. File này sẽ ghi lại toàn bộ `command` của người dùng.