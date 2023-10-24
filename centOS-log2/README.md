### Đối với tệp log có tên /var/log/messages, quyền nào được cấp cho người dùng thông thường?
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

### Trong /var/log/secure, từ ngữ nào được sử dụng để chỉ một nỗ lực đăng nhập không thành công?

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
## Liên quan đến Mục #2 ở trên, hãy mô tả một tình huống thực tế mà thông tin này có thể hữu ích.
* Để nhận biết nếu có 1 truy cập bất thường vào hệ thống
## Trong /var/log/secure, từ ngữ nào được sử dụng để chỉ rằng sinh viên đã tăng đặc quyền bằng lệnh su?
```c
Oct 24 10:32:37 logger sudo:     Joe : TTY=pts/1 ; PWD=/home/Joe ; USER=root ; COMMAND=/usr/bin/su
Oct 24 10:32:37 logger su: pam_unix(su:session): session opened for user root by Joe(uid=0)
```
## Hãy mô tả chức năng được cung cấp bởi tùy chọn -t của lệnh last.
* Dùng để kiểm tra lịch sử đăng nhập
```c
[root@logger log]# last -f wtmp -F
root     pts/1                         Tue Oct 24 10:25:20 2023 - Tue Oct 24 10:25:20 2023  (00:00)    
root     pts/1                         Tue Oct 24 10:25:20 2023 - Tue Oct 24 10:25:20 2023  (00:00)    

wtmp begins Tue Oct 24 10:25:20 2023
```
* Với tùy chọn `-t`, có thể xem được khoảng thời gian cụ thể nào có người dùng nào đăng nhập hay không
## Quy tắc nào trong tệp cấu hình syslog sẽ phù hợp với bản ghi mà sinh viên đã gửi bằng lệnh logger (tức là một facility là "user" và một priority là "info")?
## Quy tắc nào sinh viên đã thêm để đưa các thông báo gỡ lỗi (và chỉ các thông báo gỡ lỗi) vào /var/log/mydebug?
## Sinh viên đã kiểm tra quy tắc gỡ lỗi mới như thế nào?
## Sinh viên đã sử dụng lệnh nào để thay đổi quyền trên logger để chỉ người dùng root và nhóm root mới có thể thực thi nó?
## Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ kernel với mức ưu tiên là emerg?
## Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ facility mail với mức ưu tiên là notice?
## Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ facility local6 với mức ưu tiên là err?## Mô tả bất kỳ thử nghiệm hoặc khám phá bổ sung nào sinh viên đã thực hiện. 
## Sinh viên đã học được điều gì từ bài thực hành này?
## Cần làm gì để cải thiện bài thực hành này?
