### 1. Đối với tệp log có tên /var/log/messages, quyền nào được cấp cho người dùng thông thường?
```c
[root@logger log]# ll
total 444
drwxr-xr-x 2 root root   4096 Mar  2  2018 anaconda
-rw------- 1 root utmp      0 Mar  8  2018 btmp
-rw-r--r-- 1 root root    193 Mar  2  2018 grubby_prune_debug
-rw-r--r-- 1 root root 292292 Oct 24 10:26 lastlog
-rw------- 1 root root      0 Mar  8  2018 maillog
-rw------- 1 root root 126657 Oct 24 10:27 messages
drwxr-xr-x 2 root root   4096 Mar  2  2018 rhsm
-rw------- 1 root root   2988 Oct 24 10:26 secure
-rw------- 1 root root      0 Mar  8  2018 spooler
-rw------- 1 root root      0 Mar  2  2018 tallylog
-rw-rw-r-- 1 root utmp   1536 Oct 24 10:25 wtmp
-rw------- 1 root root   9660 Jan 24  2020 yum.log
```
* Quyền được sử dụng cho người dùng thông thường với file `messages` là `-rw-------` tức là root sẽ có quyền read write, với group sẽ không có quyền nào, và tương tự với other(người dùng khác) cũng vậy

### 2. Trong /var/log/secure, từ ngữ nào được sử dụng để chỉ một nỗ lực đăng nhập không thành công?

* Với nhập tên người dùng sai:
```c
Oct 24 10:32:18 logger login[416]: pam_unix(login:auth): authentication failure; logname=Joe uid=0 euid=0 tty=/dev/pts/1 ruser= rhost=
Oct 24 10:32:20 logger login[416]: FAILED LOGIN (1) on '/dev/pts/1' FOR 'UNKNOWN', Authentication failure
```
* Với nhập mật khẩu sai:
```c
Oct 24 10:32:24 logger login[416]: pam_unix(login:auth): authentication failure; logname=Joe uid=0 euid=0 tty=/dev/pts/1 ruser= rhost=  user=Joe
Oct 24 10:32:27 logger login[416]: FAILED LOGIN (2) on '/dev/pts/1' FOR 'Joe', Authentication failure
```
### 3. Liên quan đến Mục #2 ở trên, hãy mô tả một tình huống thực tế mà thông tin này có thể hữu ích.
* Để nhận biết nếu có 1 truy cập bất thường vào hệ thống
### 4. Trong /var/log/secure, từ ngữ nào được sử dụng để chỉ rằng sinh viên đã tăng đặc quyền bằng lệnh su?
```c
Oct 24 10:32:37 logger sudo:     Joe : TTY=pts/1 ; PWD=/home/Joe ; USER=root ; COMMAND=/usr/bin/su
Oct 24 10:32:37 logger su: pam_unix(su:session): session opened for user root by Joe(uid=0)
```
### 5. Hãy mô tả chức năng được cung cấp bởi tùy chọn -t của lệnh last.
* Dùng để kiểm tra lịch sử đăng nhập
```c
[root@logger log]# last -f wtmp -F
root     pts/1                         Tue Oct 24 10:25:20 2023 - Tue Oct 24 10:25:20 2023  (00:00)    
root     pts/1                         Tue Oct 24 10:25:20 2023 - Tue Oct 24 10:25:20 2023  (00:00)    

wtmp begins Tue Oct 24 10:25:20 2023
```
* Với tùy chọn `-t`, có thể xem được khoảng thời gian cụ thể nào có người dùng nào đăng nhập hay không, ví dụ:
```c
[root@logger log]# last -t 20231024102520
root     pts/1                         Tue Oct 24 10:25 - 10:25  (00:00)    
root     pts/1                         Tue Oct 24 10:25 - 10:25  (00:00)    

wtmp begins Tue Oct 24 10:25:20 2023
```
### 6. Quy tắc nào trong tệp cấu hình syslog sẽ phù hợp với bản ghi mà sinh viên đã gửi bằng lệnh logger (tức là một facility là "user" và một priority là "info")?
* Sau khi cấu hình lại tệp `rsyslog.conf`, theo dõi lại file messages ta thu được:
```c
[root@logger log]# tail -f /var/log/messages 
Oct 24 11:11:41 logger kernel: IPv6: ADDRCONF(NETDEV_UP): ens33: link is not ready
Oct 24 11:11:41 logger kernel: IPv6: ADDRCONF(NETDEV_UP): ens33: link is not ready
Oct 24 11:11:41 logger kernel: e1000: ens33 NIC Link is Up 1000 Mbps Full Duplex, Flow Control: None
Oct 24 11:11:41 logger kernel: IPv6: ADDRCONF(NETDEV_CHANGE): ens33: link becomes ready
Oct 24 11:22:21 logger kernel: perf: interrupt took too long (26448 > 25000), lowering kernel.perf_event_max_sample_rate to 7500
Oct 24 11:28:20 logger systemd: Stopping System Logging Service...
Oct 24 11:28:20 logger rsyslogd: [origin software="rsyslogd" swVersion="8.24.0" x-pid="158" x-info="http://www.rsyslog.com"] exiting on signal 15.
Oct 24 11:28:20 logger systemd: Starting System Logging Service...
Oct 24 11:28:20 logger rsyslogd: [origin software="rsyslogd" swVersion="8.24.0" x-pid="897" x-info="http://www.rsyslog.com"] start
Oct 24 11:28:20 logger systemd: Started System Logging Service.
Oct 24 11:29:20 logger rsyslogd: -- MARK --
```
* Quy tắc được đưa vào
```bash 
if $syslogseverity-text == 'info' then /var/log/messages
```

* Kết quả khi check log ở `/var/log/messages`
```c
Oct 24 12:08:21 logger rsyslogd: -- MARK --
Oct 24 12:08:38 logger Joe: Hello World
```
### 7. Quy tắc nào sinh viên đã thêm để đưa các thông báo gỡ lỗi (và chỉ các thông báo gỡ lỗi) vào /var/log/mydebug?
* Quy tắc được thêm vào file `rsyslog.conf`
```bash
if $syslogseverity-text == 'debug' then /var/log/mydebug
```

* Sau khi thêm rule thành công, sẽ xuất hiện file mydebug ở /var/log/
```c
[root@logger log]# ll
total 464
drwxr-xr-x 2 root root   4096 Mar  2  2018 anaconda
-rw------- 1 root utmp    768 Oct 24 10:32 btmp
-rw-r--r-- 1 root root    193 Mar  2  2018 grubby_prune_debug
-rw-r--r-- 1 root root 292292 Oct 24 10:40 lastlog
-rw------- 1 root root      0 Mar  8  2018 maillog
-rw------- 1 root root 134269 Oct 24 12:16 messages
-rw------- 1 root root     68 Oct 24 12:14 mydebug
drwxr-xr-x 2 root root   4096 Mar  2  2018 rhsm
-rw------- 1 root root   4298 Oct 24 10:40 secure
-rw------- 1 root root      0 Mar  8  2018 spooler
-rw------- 1 root root      0 Mar  2  2018 tallylog
-rw-rw-r-- 1 root utmp   1536 Oct 24 10:25 wtmp
-rw------- 1 root root   9660 Jan 24  2020 yum.log
```
### 8. Sinh viên đã kiểm tra quy tắc gỡ lỗi mới như thế nào?
```c
[root@logger log]# logger -p debug "Debug sieu cap vu tru"
[root@logger log]# cat mydebug 
Oct 24 12:14:27 logger Joe: hello
Oct 24 12:14:33 logger Joe: hello
Oct 24 12:18:08 logger Joe: Debug sieu cap vu tru
```
### 9. Sinh viên đã sử dụng lệnh nào để thay đổi quyền trên logger để chỉ người dùng root và nhóm root mới có thể thực thi nó?
### 10. Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ kernel với mức ưu tiên là emerg?
### 11. Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ facility mail với mức ưu tiên là notice?
### 12. Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ facility local6 với mức ưu tiên là err?## Mô tả bất kỳ thử nghiệm hoặc khám phá bổ sung nào sinh viên đã thực hiện. 
### 13. Sinh viên đã học được điều gì từ bài thực hành này?
### 14. Cần làm gì để cải thiện bài thực hành này?
