Bài thực hành: Khám phá Nhật ký (Log) Unix trên CentOS
1.	Mục đích
Mục tiêu của bài tập này là để cung cấp cho sinh viên một trải nghiệm thực tế với cấu hình và kiểm thử syslog.
2.	Yêu cầu đối với sinh viên:
Nắm được kiến thức về CentOS và syslog.
3.    Nội dung thực hành
Chuẩn bị lab
-   	Khởi động lab:
labtainer centos-log2
(chú ý: sinh viên sử dụng tên tài khoản của mình để nhập thông tin email người thực hiện bài lab khi có yêu cầu trong terminal, để sử dụng khi chấm điểm. Thông thường tên tài khoản của sinh viên chính là Mã sinh viên)

Các nhiệm vụ
Đăng nhập vào CentOS với tên người dùng Joe và mật khẩu "password4joe".
Nhiệm vụ 1: Khám phá
1. Trong terminal, nhập lệnh sudo su nhưng nhập sai mật khẩu cho người dùng root.
2. Nhập lại lệnh sudo su, nhưng lần này nhập đúng mật khẩu cho root. Nếu làm đúng, dấu nhắc sẽ kết thúc bằng ký tự '#'.
3. Khám phá thư mục log
   - Thay đổi thư mục làm việc hiện tại thành /var/log.
   - Liệt kê nội dung của /var/log.
drwxr-xr-x 2 root root   4096 Mar  2  2018 anaconda
-rw------- 1 root utmp      0 Mar  8  2018 btmp
-rw-r--r-- 1 root root    193 Mar  2  2018 grubby_prune_debug
-rw-r--r-- 1 root root 292292 Oct 24 02:51 lastlog
-rw------- 1 root root      0 Mar  8  2018 maillog
-rw------- 1 root root 109226 Oct 24 02:51 messages
drwxr-xr-x 2 root root   4096 Mar  2  2018 rhsm
-rw------- 1 root root   2988 Oct 24 02:51 secure
-rw------- 1 root root      0 Mar  8  2018 spooler
-rw------- 1 root root      0 Mar  2  2018 tallylog
-rw-rw-r-- 1 root utmp   1536 Oct 24 02:50 wtmp
-rw------- 1 root root   9660 Jan 24  2020 yum.log
•	Sinh viên sẽ thấy nhiều tệp và thư mục khác nhau. Lưu ý rằng tên màu xanh đề cập đến thư mục. Sinh viên có thể thấy một số "phần mở rộng" (extensions) khác nhau trên các tệp:
     + .old: Đây là bản sao "xoay vòng" (rotated) của nhật ký. Sinh viên sẽ thấy một tệp khác có cùng tiền tố, nhưng không có phần mở rộng ".old".
     + -yyyymmdd: Đây là một ví dụ khác về nhật ký "xoay vòng" (rotated) nhưng có một phần mở rộng hữu ích hơn: ngày mà nó đã được xoay vòng. Nếu sinh viên thấy phần mở rộng này, thì sinh viên cũng sẽ thấy một tệp khác có cùng tiền tố, nhưng không có phần mở rộng về ngày tháng.
   - Xem quyền truy cập của messages log.
   - Ghi lại trong Mục số #1 của báo cáo quyền hạn mà người dùng thông thường có đối với tệp này. rw-------
   - Đa số các tệp trong thư mục nhật ký là tệp văn bản, nhưng cũng có những ngoại lệ so với quy tắc thông thường của Unix. Nhiều tệp sẽ trống, hoặc là do tệp này vừa được xoay vòng, hoặc là do dịch vụ liên quan không hoạt động, hoặc chưa phát hiện được sự kiện có thể kiểm tra được liên quan đến dịch vụ đó.
4. Mật khẩu sai
   - Các bản ghi liên quan đến đăng nhập được lưu trong tệp văn bản có tên là secure. Các bản ghi mới nhất được ghi vào cuối tệp.
   - Mở tệp và tìm kiếm trạng thái failed khi cố gắng đăng nhập bằng tên người dùng Joe (không phải sự thất bại khi 'su' thành root).
   - Trong Mục số #2 của báo cáo, ghi lại cụm từ được sử dụng để chỉ ra sự thất bại trong việc đăng nhập.
Oct 24 03:04:06 logger login[1197]: pam_unix(login:auth): authentication failure; logname= uid=0 euid=0 tty=/dev/pts/2 ruser= rhost=
Oct 24 03:04:08 logger login[1197]: FAILED LOGIN (1) on '/dev/pts/2' FOR 'UNKNOWN', Authentication failure
   - Lưu ý rằng Mục số #3 cần trả lời các câu hỏi bổ sung.
5. Mật khẩu là tên người dùng
   - Với tệp nhật ký secure vẫn mở, tìm dòng ghi chú cho biết sinh viên đã nhập "password" làm tên người dùng.
   - Trong Mục số #4 của báo cáo, ghi lại cụm từ được sử dụng khi bạn nhập một tên người dùng không hợp lệ.
Oct 24 03:04:13 logger login[1197]: pam_unix(login:auth): authentication failure; logname= uid=0 euid=0 tty=/dev/pts/2 ruser= rhost=  user=Joe
Oct 24 03:04:14 logger login[1197]: FAILED LOGIN (2) on '/dev/pts/2' FOR 'Joe', Authentication failure
6. Sử dụng su
   - Với tệp nhật ký secure vẫn mở, tìm mục ở cuối tệp liên quan đến hành động su thành root trước đó. Xem thông tin được lưu trữ về sử dụng su.
   - Trong Mục số #5 của báo cáo, ghi lại thông tin đã được ghi lại về sử dụng su   
Oct 24 03:04:45 logger sudo:     Joe : TTY=pts/2 ; PWD=/home/Joe ; USER=root ; COMMAND=/usr/bin/su
Oct 24 03:04:45 logger su: pam_unix(su:session): session opened for user root by Joe(uid=0)
   - Thoát khỏi trình soạn thảo khi sinh viên đã hoàn thành.
7. Tệp wtmp
   - Một trong số các tệp nhị phân trong thư mục nhật ký là tệp wtmp phổ biến, yêu cầu sử dụng các công cụ khác để trích xuất thông tin từ nó, chẳng hạn như lệnh last.
   - Mở trang hỗ trợ “man” cho lệnh last bằng cách thực hiện các bước sau:
man last
   - Đọc phần DESCRIPTION để tìm hiểu chức năng của lệnh.
   - Điều hướng đến phần OPTIONS.
   - Trong Mục số #6 của báo cáo tự mô tả và giải thích lựa chọn –t của lệnh last.
Nhiệm vụ 2: Cấu hình lại rsyslog cho MARK
Trong phần này, sinh viên sẽ bật tính năng MARK và khởi động lại syslog để chấp nhận thay đổi.
1. Mở tệp cấu hình rsyslog.
Trong khi vẫn chạy với đặc quyền root trong terminal, khởi chạy một trình soạn thảo từ dòng lệnh (như leafpad) để mở tệp /etc/rsyslog.conf
Lưu ý: khi tiến trình rsyslog đọc tệp này trong quá trình khởi tạo, bất cứ điều gì sau ký tự '#' (từ đó đến cuối dòng) được xem như một chú thích.
2. Bật tính năng Mark.
Mặc định, việc chèn thời gian vào một tần suất đã chỉ định được vô hiệu hóa.
- Trong phần "### MODULES ###", tìm dòng có $ModLoad immark, và xóa ‘#’ để kích hoạt tính năng này.
- Thiết lập tần suất của timestamps với việc thêm dòng tiếp theo dòng bên trên vừa mới thêm vào:
$MarkMessagePeriod 60
	“60” là số giây giữa các timestamps (giá trị mặc định thường là 20 phút)
- Lưu thay đổi và thoát khỏi trình soạn thảo.
3. Khởi động lại tiến trình rsyslog.
Khởi động lại tiến trình rsyslog sẽ khiến nó khởi tạo lại và đọc lại tệp cấu hình (đồng nghĩa với việc thay đổi được áp dụng). Thực hiện các bước sau để khởi động lại:
  service rsyslog restart
4. Xem thay đổi này đã được thực hiện trong các nhật ký bằng cách sử dụng lệnh tail như sau:
  tail -f /var/log/messages
Lệnh tail hiển thị một số dòng cuối cùng của tệp (khác với lệnh head, hiển thị một số dòng đầu tiên của tệp). Tùy chọn "-f" cho biết để chờ đợi “mãi mãi” và hiển thị thêm dòng khi chúng được thêm vào cuối tệp.
Sinh viên sẽ thấy một dòng ghi lại việc dừng rsyslogd, và sau đó là một dòng ghi lại rằng rsyslogd đã được khởi động.
Tiếp tục chờ đợi cho đến khi thấy một bản ghi MARK xuất hiện trong nhật ký. Sau khi sinh viên đã thấy nó (hoặc sau hơn một phút), nhấn Ctrl-C để thoát khỏi tail.

Nhiệm vụ 3: Cấu hình lại và Kiểm tra rsyslog
Trong phần này, sinh viên sẽ làm quen với tiện ích logger để tạo thủ công các mục syslog. Một quản trị viên hệ thống có thể sử dụng lệnh này để ghi lại các thay đổi mà họ thực hiện trên hệ thống, và nó có thể được sử dụng để kiểm tra các thay đổi trong cấu hình syslog. Sinh viên sẽ thực hiện một số thay đổi trong các quy tắc syslog, sau đó sử dụng logger để kiểm tra các thay đổi đó.
1. Đọc phần DESCRIPTION trong trang man của tiện ích logger:
man logger
2. Tạo một mục trong /var/log/messages với mức ưu tiên "info" bằng cách thực hiện các bước sau:
logger -p info "Hello World"
Khi không chỉ định cơ sở dữ liệu, như trong trường hợp của lệnh trên, cơ sở dữ liệu "user" được sử dụng mặc định.
3. Mở lại tệp cấu hình rsyslog tại /etc/rsyslog.conf và cuộn xuống phần “#### RULES ####”. 
Trong Mục số #7 của báo cáo sinh viên hãy viết quy tắc syslog chỉ định điều gì sẽ xảy ra với mục mà sinh viên đã gửi đến syslog trong bước #2 ở trên.
4. Thoát khỏi trình soạn thảo.
5. Sử dụng grep (hoặc chọn công cụ khác) để xác minh rằng mục nhật ký đã được lưu trong tệp mà sinh viên nghĩ rằng nó sẽ được lưu (theo quy tắc sinh viên ghi lại trong mục số #7 của báo cáo). [Nếu không có trong đó, thì sinh viên đã làm sai. Trong trường hợp đó, hãy xem xét lại quy tắc sinh viên chọn cho đến khi sinh viên đạt được đúng.]
6. Mở lại tệp cấu hình syslog và cuộn xuống phần RULES.
Thêm một quy tắc syslog mới để đưa tất cả các thông báo với mức ưu tiên "debug" vào một tệp có tên là /var/log/mydebug. Tệp này chỉ nên chứa các thông báo debug. 
Trong Mục số #8 của báo cáo, sinh viên hãy viết quy tắc đã sử dụng để đáp ứng yêu cầu trên.
7. Lưu các thay đổi của bạn vào tệp cấu hình và sau đó thoát khỏi trình soạn thảo.
8. Khởi động lại rsyslog (để quy tắc mới có hiệu lực):
systemctl restart rsyslog
Nếu thay đổi trong rsyslog.conf có lỗi cú pháp, nó sẽ được báo cáo ở cuối tệp /var/log/messages.
9. Bây giờ sinh viên đã biết cách sử dụng logger, hãy sử dụng nó để kiểm tra quy tắc mà sinh viên đã thêm vào rsyslog.conf ở bước #6 ở trên.
Trong Mục số #9 của báo cáo, mô tả cách sinh viên đã sử dụng logger (và các lệnh khác) để kiểm tra quy tắc sinh viên đã thêm trong bước #6.
10. Thực hiện các bước sau để hiển thị quyền liên quan đến lệnh logger:
ll/bin/logger
Không nên cho phép người dùng thông thường thực thi lệnh logger. Thay đổi quyền sao cho chỉ người dùng root và nhóm root mới có thể thực thi nó.
Trong Mục số #10 của báo cáo, viết lệnh (hoặc các lệnh) sinh viên đã sử dụng để thay đổi quyền trên lệnh logger.
Nhiệm vụ 4: Ghi log tập trung
Giả sử sinh viên có một số hệ thống Linux cần quản lý. Thay vì cấu hình và xem xét việc ghi log trên từng hệ thống, sinh viên có thể xác định một hệ thống ghi log tập trung và sau đó chuyển tiếp các thông báo log từ mỗi hệ thống đến hệ thống ghi log tập trung đó. Ở phần này, sinh viên sẽ cấu hình hệ thống "logger" hiện có để chấp nhận các thông báo log từ các máy tính từ xa, và sinh viên sẽ cấu hình một máy tính trạm để chuyển tiếp các log của nó đến hệ thống ghi log.
1.	Mở lại tệp cấu hình /etc/rsyslog.conf trên máy tính ghi log.
2.	Tìm các mục sau trong tệp cấu hình và bỏ chú thích chúng (xóa dấu "#") để cho phép chấp nhận thông báo syslog trên cổng 514 qua TCP hoặc UDP: 
$ModLoad imudp
$UDPServerRun 514
$ModLoad imtcp
$InputTCPServerRun 514
3.	Khởi động lại rsyslog
systemctl restart rsyslog
4.	Trên terminal chính của hệ thống lab sử dụng lệnh:
moreterm.py centos-log2 workstation
5.	Một terminal ảo mới được mở và kết nối với máy tính trạm. Máy tính này chia sẻ mạng với máy tính ghi log của sinh viên. Sử dụng "ifconfig" trên mỗi máy tính để xem địa chỉ IP của mỗi máy tính.
6.	Trên máy tính ghi log, sử dụng "tail" để xem các nhật ký:
tail -f /var/log/*
7.	Sử dụng "sudo su" để nâng cao đặc quyền trên máy tính trạm.
8.	Mở tệp /etc/rsyslog.conf trên máy tính trạm và tìm phần "RULES". Ở cuối phần đó, thêm dòng sau để chuyển hướng tất cả các thông báo đến máy tính ghi log:
*.* @172.25.0.2
9.	Bây giờ hãy khởi động lại rsyslog trên máy tính trạm và quan sát các thông báo log trên máy tính ghi log.
10.	Thử nghiệm với các sự kiện liên quan đến bảo mật khác nhau như giảm đặc quyền và nâng cao đặc quyền trên máy tính trạm và thực hiện các lệnh logger từ máy tính trạm.
Nhiệm vụ 5: Các câu hỏi khác
1.	 Mở lại /etc/rsyslog.conf.
Tham khảo /etc/rsyslog.conf, trả lời các câu hỏi trong các mục #11 đến #13 của báo cáo.
2. Mô tả hành động và kết quả trong Mục số #14 của báo cáo.
3. Trong Mục #15 của báo cáo hãy mô tả những điều sinh viên đã học từ bài thực hành này.
14. Trong Mục #16 của báo cáo hãy mô tả bất kỳ đề xuất nào có để cải thiện bài thực hành này.
Kết thúc bài lab:
-   	Trên terminal đầu tiên sử dụng câu lệnh sau để kết thúc bài lab:
stoplab
-   	Khi bài lab kết thúc, một tệp zip lưu kết quả được tạo và lưu vào một vị trí được hiển thị bên dưới lệnh stoplab.
Khởi động lại bài lab:
Trong quá trình làm bài sinh viên cần thực hiện lại bài lab, dùng câu lệnh:
labtainer –r centos-log






Một vài lệnh Unix hữu ích
cd	Change directory
  cd location
With no “location”, you will be taken to your home directory.
chmod	Change the DAC permissions on a file or directory.
  chmod permissions objectname
clear	Erase all the output on the current terminal and place the shell prompt at the top of the terminal.
grep	Search for a string in the given input.
  grep string filename
The above command will search for “string” within the given file. If the string has spaces in it, then it must be quoted.
ls	List the contents and/or attributes of a directory or file
  ls location
 ls file
With no “location” or “file” it will display the contents of the current working directory.
less	Display a page of a text file at a time in the terminal
  more file
To see another page press the space bar. To see one more line press the Enter key. To quit before reaching the end of the file enter ‘q’.
man	Manual
  man command
Displays the manual page for the given “command”. To see another page press the space bar. To see one more line press the Enter key. To quit before reaching the end of the file enter ‘q’.
more	Display a page of a text file at a time in the terminal
  more file
To see another page press the space bar. To see one more line press the Enter key. To quit before reaching the end of the file enter ‘q’.
pwd	Display the present working directory
  pwd
su	Super user (change to root)
tail	Display the last 20 lines of a text file on the terminal.
touch	Change the modification date on the given file. If the file does not exist, it will be created.
  touch filename
If no time is provided, then the modification time will be change to the current time. 
Given the right options, touch can also be used to change the modification to a specific date/time.

Sinh viên cần trả lời các câu hỏi sau và đưa vào báo cáo
1.	Đối với tệp log có tên /var/log/messages, quyền nào được cấp cho người dùng thông thường?
2.	Trong /var/log/secure, từ ngữ nào được sử dụng để chỉ một nỗ lực đăng nhập không thành công?
3.	Liên quan đến Mục #2 ở trên, hãy mô tả một tình huống thực tế mà thông tin này có thể hữu ích.
4.	Trong /var/log/secure, từ ngữ nào được sử dụng để chỉ rằng sinh viên đã tăng đặc quyền bằng lệnh su?
5.	Hãy mô tả chức năng được cung cấp bởi tùy chọn -t của lệnh last.
6.	Quy tắc nào trong tệp cấu hình syslog sẽ phù hợp với bản ghi mà sinh viên đã gửi bằng lệnh logger (tức là một facility là "user" và một priority là "info")?
7.	Quy tắc nào sinh viên đã thêm để đưa các thông báo gỡ lỗi (và chỉ các thông báo gỡ lỗi) vào /var/log/mydebug?
8.	Sinh viên đã kiểm tra quy tắc gỡ lỗi mới như thế nào?
9.	Sinh viên đã sử dụng lệnh nào để thay đổi quyền trên logger để chỉ người dùng root và nhóm root mới có thể thực thi nó?
10.	 Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ kernel với mức ưu tiên là emerg?
11.	 Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ facility mail với mức ưu tiên là notice?
12.	 Nhìn vào tất cả các quy tắc hoạt động trong rsyslog.conf, hành động nào sẽ rsyslog thực hiện nếu nhận được một bản ghi từ facility local6 với mức ưu tiên là err?
13.	 Mô tả bất kỳ thử nghiệm hoặc khám phá bổ sung nào sinh viên đã thực hiện. 
14.	 Sinh viên đã học được điều gì từ bài thực hành này?
15.	 Cần làm gì để cải thiện bài thực hành này?
