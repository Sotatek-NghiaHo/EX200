# CHAPTER 1 : Lab: Improve Command-line Productivity

Tạo một tập lệnh Bash có thể lọc và lấy thông tin liên quan từ các máy chủ khác nhau.

Kết quả
- Tạo một tập lệnh Bash và chuyển hướng đầu ra của nó vào một tệp.
- Sử dụng vòng lặp để đơn giản hóa mã của bạn.
- Lọc nội dung liên quan bằng cách sử dụng grep và biểu thức chính quy.

1. Tạo tệp lệnh thực thi `/home/student/bin/bash-lab` trên máy `workstation` . Nội dung ban đầu trong lệnh phải sử dụng chỉ thị thông dịch shebang.

Kiểm tra Bash shell nằm ở đâu bằng lệnh `which` hoặc `type`
```
[root@redhat9-server-1 ~]# which bash
/usr/bin/bash
[root@redhat9-server-1 ~]# type -a bash
bash is /usr/bin/bash

```
On the workstation machine, create the `/home/student/bin/` directory if needed.
```
[student@workstation ~]$ mkdir -p /home/student/bin
```
Use the vim command to create and edit the /home/student/bin/bash-lab script file.
```
[student@workstation ~]$ vim ~/bin/bash-lab
```
Insert the following text and save the file.
```
#!/usr/bin/bash
```
Make your script file executable.
```
[student@workstation ~]$ chmod a+x ~/bin/bash-lab
```
So sánh `chmod a+x` & `chmod +x`
| Lệnh | Ai được quyền thự thi?  |
| --- | --- |
| `chmod +x file`  | Thường là owner, có thể nhóm/others tùy umask |
| `chmod a+x file` | Owner, nhóm và mọi người khác **chắc chắn**   |

![](../pic/38.png)

2. Chỉnh sửa tệp script mới tạo của bạn để lưu trữ *thông tin sau* từ máy servera và serverb trên máy trạm. Hệ thống sử dụng khóa SSH để xác thực, do đó bạn không cần mật khẩu. Lưu trữ kết quả đầu ra của các lệnh được liệt kê từ bảng sau vào các tệp `/home/student/output-servera` và `/home/student/output-serverb` tương ứng trên máy trạm. Sử dụng dấu thăng (#) để phân biệt kết quả đầu ra của các lệnh liên tiếp trong tệp đầu ra.

Command or file	| Content requested | Muc tieu
---|---|---
hostname -f | Lưu trữ toàn bộ đầu ra| tên đầy đủ của host (hostname)
echo "#####"| Thêm dấu thăng để phân biệt lệnh sau
lscpu| Chỉ lấy các dòng bắt đầu bằng chuỗi CPU| Lấy thông tin CPU  
echo "#####" | Thêm dấu thăng để phân biệt lệnh sau.
/etc/selinux/config | Bỏ qua các dòng trống. Ngoài ra, bỏ qua các dòng bắt đầu bằng ký tự #. | Kiểm tra cấu hình SELinux
echo "#####" | Thêm dấu thăng để phân biệt lệnh sau.  
/var/log/secure | Lấy tất cả các mục "Mật khẩu không hợp lệ". | Kiểm tra log thất bại đăng nhập
echo "#####" |Thêm dấu thăng để phân biệt lệnh sau.

Lưu thông tin cần thiết vào các tệp `output-servera` và `output-serverb` trong thư mục `/home/student` trên máy workstation.

Note: Bạn có thể sử dụng lệnh sudo mà không cần mật khẩu trên máy chủ servera và serverb. Hãy nhớ sử dụng vòng lặp để đơn giản hóa tập lệnh của bạn. Bạn cũng có thể sử dụng nhiều lệnh grep được nối với nhau bằng cách sử dụng ký tự ống (|).

2.1 
```
[student@workstation ~]$ vim ~/bin/bash-lab
```
2.2 
```
#!/usr/bin/bash
USR='student'
OUT='/home/student/output'
#
for SRV in servera serverb; do
  ssh ${USR}@${SRV} "hostname -f" > ${OUT}-${SRV}
  echo "#####" >> ${OUT}-${SRV}
  ssh ${USR}@${SRV} "lscpu | grep '^CPU'" >> ${OUT}-${SRV}
  echo "#####" >> ${OUT}-${SRV}
  ssh ${USR}@${SRV} "grep -v '^$' /etc/selinux/config|grep -v '^#'" >> ${OUT}-${SRV}
  echo "#####" >> ${OUT}-${SRV}
  ssh ${USR}@${SRV} "sudo grep 'Failed password' /var/log/secure" >> ${OUT}-${SRV}
  echo "#####" >> ${OUT}-${SRV}
done
```
Giai thich:  

- Lệnh lscpu in thông tin CPU, `grep '^CPU' `lọc những dòng bắt đầu bằng "CPU".  

`ssh ${USR}@${SRV} "lscpu | grep '^CPU'" >> ${OUT}-${SRV}`    
- Lọc file /etc/selinux/config:  

`ssh ${USR}@${SRV} "grep -v '^$' /etc/selinux/config | grep -v '^#'" >> ${OUT}-${SRV}`

- grep -v '^$' → bỏ dòng trống
- grep -v '^#' → bỏ dòng comment

Note: Cau hinh user NOPASSWD  
![](../pic/41.png)

Tuy chon -i khi ssh  
![](../pic/39.png)

Co che timestamp khi nhap pass   
![](../pic/40.png)   

Neu script khong co quyen +x bi loi  
```
[root@redhat9-server-1 ~]# /root/bash-lab
-bash: /root/bash-lab: Permission denied
[root@redhat9-server-1 ~]# ll
total 8
-rw-------. 1 root root 836 Aug  5 09:55 anaconda-ks.cfg
-rw-r--r--. 1 root root 492 Aug 15 11:28 bash-lab
```
Trong script user student/ nghiahv phai co quyen sudo & no password
```
[root@redhat9-server-1 ~]# ll
total 8
-rw-------. 1 root root 836 Aug  5 09:55 anaconda-ks.cfg
-rwxr-xr-x. 1 root root 492 Aug 15 11:28 bash-lab
[root@redhat9-server-1 ~]# /root/bash-lab
sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
sudo: a password is required

```
Có 3 hướng xử lý khi chay file script
- Chạy trực tiếp bằng đường dẫn tuyệt đối  
`/root/bash-lab`
- Chạy bằng đường dẫn tương đối (nếu đang đứng ở /root)   
`./bash-lab`
- Di chuyển file về thư mục trong $PATH 
```
[root@redhat9-server-1 ~]# echo $PATH
/root/.local/bin:/root/bin:/usr/local/sbin:/sbin:/bin:/usr/sbin:/usr/bin
```
Khi bạn gõ một lệnh (ví dụ `bash-lab)`), shell sẽ tìm file thực thi theo thứ tự các thư mục liệt kê trong biến môi trường `$PATH`.
```
# neu ko co thu muc trong $PATH tren thi tao
mkdir -p /root/bin
mv /root/bash-lab /root/bin/
chmod +x /root/bin/bash-lab
bash-lab
```

![](../pic/42.png)

Note:
1. `sh filename.sh`
- Bạn chỉ định rõ chạy script bằng sh (thường là /bin/sh).
- Nội dung file sẽ được sh đọc và thực thi, không cần quyền thực thi (+x).
- Không quan tâm shebang ở đầu file, vì bạn đã chỉ định trình thông dịch là sh.
- Nếu script viết cho bash mà dùng sh (nhất là trên Debian/Ubuntu, sh = dash), một số câu lệnh nâng cao của bash có thể lỗi.

2. `filename.sh` (hoặc bash-lab, không cần .sh)
- Hệ thống sẽ dựa vào shebang ở dòng đầu tiên (#!...) để biết dùng trình thông dịch nào (/usr/bin/bash, /usr/bin/python, …).
- File bắt buộc phải có quyền thực thi (chmod +x filename.sh).
- Nếu không có shebang, shell mặc định (thường là bash khi bạn đăng nhập) sẽ xử lý file.
- Nam trong $PATH (neu muon go ten file ma khong can duong dan)

# CHAPTER 3: Lab: Analyze and Store Logs
Thay đổi múi giờ trên máy chủ hiện có và cấu hình tệp nhật ký mới cho tất cả các sự kiện xác thực không thành công.

Kết quả
- Cập nhật múi giờ trên máy chủ hiện có.
- Cấu hình tệp nhật ký mới để lưu trữ tất cả thông báo xác thực không thành công.

2. Giả sử máy chủ được chuyển đến Jamaica và bạn phải cập nhật múi giờ thành `America/Jamaica`. Hãy kiểm tra xem bạn đã đặt đúng múi giờ chưa.  

2.1 Chọn múi giờ phù hợp cho Jamaica.
```
[student@serverb ~]$ tzselect
-> 2 (Americas)
-> 30 (Jamaica)
-> 1 (Yes)
```
Note:   
- `tzselect` → chỉ để chọn và hiển thị tên múi giờ.
- `timedatectl set-timezone ...` → mới là lệnh áp dụng thật.

2.2 Nâng cao quyền của người dùng `student` để cập nhật múi giờ của máy chủ serverb thành America/Jamaica.
```
[student@serverb ~]$ sudo timedatectl set-timezone America/Jamaica
[sudo] password for student: student
```
2.3 Verify that you successfully set the time zone to America/Jamaica.
```
[student@serverb ~]$ timedatectl
               Local time: Wed 2022-03-16 07:18:40 EST
           Universal time: Wed 2022-03-16 12:18:40 UTC
                 RTC time: Wed 2022-03-16 12:18:40
                Time zone: America/Jamaica (EST, -0500)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

3. Xem các log events được ghi lại trong 30 phút trước trên máy chủ serverb.
```
[student@serverb ~]$ journalctl --since "-30min"
...output omitted...
Mar 16 07:10:58 localhost kernel: x86/PAT: Configuration [0-7]: WB  WC  UC- UC  WB  WP  UC- WT
Mar 16 07:10:58 localhost kernel: found SMP MP-table at [mem 0x000f5bd0-0x000f5bdf]
Mar 16 07:10:58 localhost kernel: Using GB pages for direct mapping
Mar 16 07:10:58 localhost kernel: RAMDISK: [mem 0x2e0d9000-0x33064fff]
Mar 16 07:10:58 localhost kernel: ACPI: Early table checksum verification disabled
Mar 16 07:10:58 localhost kernel: ACPI: RSDP 0x00000000000F5B90 000014 (v00 BOCHS )
Mar 16 07:10:58 localhost kernel: ACPI: RSDT 0x000000007FFE12C4 00002C (v01 BOCHS  BXPCRSDT 00000001 BXPC 00000001)
Mar 16 07:10:58 localhost kernel: ACPI: FACP 0x000000007FFE11D0 000074 (v01 BOCHS  BXPCFACP 00000001 BXPC 00000001)
Mar 16 07:10:58 localhost kernel: ACPI: DSDT 0x000000007FFDFDC0 001410 (v01 BOCHS  BXPCDSDT 00000001 BXPC 00000001)
lines 1-50/50 q
[student@serverb ~]$
```

4. Tạo tệp `/etc/rsyslog.d/auth-errors.conf`. Cấu hình dịch vụ `rsyslog` để ghi thông báo "Logging test authpriv.alert " vào tệp `/var/log/auth-errors.conf`. Sử dụng chức năng `authpriv` và mức độ ưu tiên `alert` .  
- Bạn phải tạo file cấu hình riêng /etc/rsyslog.d/auth-errors.conf để chỉ định quy tắc log.
- Quy tắc là: mọi log thuộc facility authpriv và priority alert thì ghi vào /var/log/auth-errors.
- Sau đó phải test bằng cách gửi một log test, ví dụ dùng logger.

4.1 Tạo tệp `/etc/rsyslog.d/auth-errors.conf` và chỉ định tệp `/var/log/auth-errors.conf`  làm đích đến cho các thông báo xác thực và bảo mật.

```
sudo vi /etc/rsyslog.d/auth-errors.conf
# them dong ben duoi
authpriv.alert    /var/log/auth-errors

```
📌 Giải thích:
- authpriv → facility liên quan đến xác thực (login, sudo, ssh...).
- alert → mức ưu tiên (priority) cao (chỉ log alert và cao hơn).
- /var/log/auth-errors → file bạn muốn ghi log vào.

4.2 Restart the `rsyslog` service to apply the configuration file changes.

```
[student@serverb ~]$ sudo systemctl restart rsyslog

```

4.3 Sử dụng lệnh `logger -p` để ghi thông báo "Logging test authpriv.alert" vào tệp /var/log/auth-errors. Sử dụng chức năng authpriv và mức độ ưu tiên cảnh báo.

```
[student@serverb ~]$ logger -p authpriv.alert "Logging test authpriv.alert"

```

4.4 Xác minh rằng tệp `/var/log/auth-errors` có chứa mục nhật ký với thông báo Logging test authpriv.alert hay không.

```
[student@serverb ~]$ sudo tail /var/log/auth-errors
Mar 16 07:25:12 serverb student[1339]: Logging test authpriv.alert
```

4.5  Return to the workstation system as the student user.


```
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

Facility  
![](../pic/43.png)

Priority
![](../pic/44.png)

# CHAPTER 4: Lab: Archive and Transfer Files  
Sử dụng các lệnh tar, rsync và sftp để lưu trữ và sao lưu nội dung của các thư mục.

Kết quả  
- Đồng bộ hóa thư mục từ xa với thư mục cục bộ.
- Tạo một kho lưu trữ nội dung của thư mục đã đồng bộ.
- Sao chép an toàn một kho lưu trữ sang máy chủ từ xa.
- Giải nén một kho lưu trữ.

Note:  It also installs SSH keys on your systems so that you can transfer files without entering passwords.

1. Trên serverb, đồng bộ hóa cây thư mục `/etc` từ servera sang thư mục `/configsync`.  

1.1
```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```
1.2 Tạo thư mục `/configsync` để lưu trữ các tệp được đồng bộ hóa từ servera.
```
[root@serverb ~]# mkdir /configsync

```
1.3 Đồng bộ hóa cây thư mục `/etc` từ servera đến thư mục `/configsync` trên serverb.

```
[root@serverb ~]# rsync -av root@servera:/etc /configsync
receiving incremental file list
etc/
etc/.pwd.lock
etc/.rht_authorized_keys
...output omitted...
sent 11,519 bytes  received 20,203,285 bytes  40,429,608.00 bytes/sec
total size is 20,150,298  speedup is 1.00
```
Chu thich:
Trong rsync, tùy chọn -av là viết tắt của:
- -a (archive mode)
  - Giữ nguyên toàn bộ quyền file, symlink, timestamp, owner, group…
  - Tự động bật -r (recursive) để copy thư mục và file bên trong.
  - Mục tiêu: sao lưu/dồng bộ giống hệt bản gốc.
- -v (verbose)
  - Hiển thị chi tiết quá trình copy: tên file, tiến trình, v.v.


SS `scp` va `rsync`  
![](../pic/45.png)

2. Tạo tệp lưu trữ `configfile-backup-servera.tar.gz` với nội dung thư mục `/configsync`.  

2.1 Tạo một tệp nén gzip.
```
[root@serverb ~]# tar -czf configfile-backup-servera.tar.gz /configsync
tar: Removing leading `/' from member names
```
Giai thich "Removing leading..."
![](../pic/46.png)

2.2 Liệt kê nội dung của kho lưu trữ `configfile-backup-servera.tar.gz`.
```
[root@serverb ~]# tar -tzf configfile-backup-servera.tar.gz
...output omitted...
configsync/etc/vimrc
configsync/etc/wgetrc
configsync/etc/xattr.conf
```
Note : Khi `tar -tzf`, bạn sẽ thấy đường dẫn tương đối bắt đầu bằng `configsync/`, chứ không phải tuyệt đối `/configsync/`.

Bonus: Nếu muốn file nằm chỗ khác, Nếu muốn lưu ở chỗ khác thì bạn phải ghi đường dẫn tuyệt đối hoặc tương đối:
```
tar -czf /tmp/configfile-backup-servera.tar.gz /configsync    # tuyệt đối
tar -czf ../backup/configfile-backup-servera.tar.gz /configsync   # tương đối

```

3. Sao chép an toàn tệp lưu trữ `/root/configfile-backup-servera.tar.gz` từ serverb vào thư mục `/home/student` trên máy workstation.
```
[root@serverb ~]# sftp student@workstation
student@workstation's password: student
Connected to workstation.
sftp> put configfile-backup-servera.tar.gz
Uploading configfile-backup-servera.tar.gz to /home/student/configfile-backup-servera.tar.gz
configfile-backup-servera.tar.gz                 100% 4933KB 359.5MB/s   00:00
sftp> bye
```

4. Trên workstation, giải nén nội dung vào thư mục `/tmp/savedconfig/`.

4.1 Trở lại hệ thống máy workstation với tư cách là người dùng là `student`.
```
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation]$
```
4.2 Tạo thư mục `/tmp/savedconfig `để lưu trữ nội dung đã giải nén. Chuyển sang thư mục mới.

```
[student@workstation ~]$ mkdir /tmp/savedconfig
[student@workstation ~]$ cd /tmp/savedconfig
[student@workstation savedconfig]$
```
4.3 Liệt kê nội dung của kho lưu trữ `configfile-backup-servera.tar.gz`.

```
[student@workstation savedconfig]$ tar -tzf ~/configfile-backup-servera.tar.gz
...output omitted...
configsync/etc/vimrc
configsync/etc/wgetrc
configsync/etc/xattr.conf
```
4.4 Giải nén nội dung lưu trữ vào thư mục `/tmp/savedconfig/`.

```
[student@workstation savedconfig]$ tar -xzf ~/configfile-backup-servera.tar.gz

```

Bonus: neu muon chỉ định thư mục giải nén
```
tar -xzf ~/configfile-backup-servera.tar.gz -C /tmp
```

4.5 Liệt kê thư mục để xác minh rằng nó chứa các tệp etc
 
```
[student@workstation savedconfig]$ ls -lR
.:
total 0
drwxr-xr-x. 3 student student 17 Mar 28 16:32 configsync

./configsync:
total 12
drwxr-xr-x. 105 student student 8192 Mar 28 16:03 etc
...output omitted...
```
4.6 Return to the student user's home directory.
```
[student@workstation savedconfig]$ cd
[student@workstation ~]$
```

# CHAPTER 5: Lab: Tune System Performance  
Áp dụng một cấu hình điều chỉnh cụ thể và điều chỉnh mức độ ưu tiên lập lịch của một tiến trình hiện có đang sử dụng nhiều CPU.   
Kết quả   
- Kích hoạt một cấu hình điều chỉnh cụ thể cho một hệ thống máy tính. 
- Điều chỉnh mức độ ưu tiên lập lịch CPU của một tiến trình.

1. Thay đổi cấu hình điều chỉnh hiện tại cho máy chủ `serverb` thành `balanced` profile, một cấu hình điều chỉnh chung không chuyên biệt. Liệt kê thông tin cho `balanced` tuning profile khi đó là tuning profile hiện tại.

1.1 Đăng nhập vào máy chủ `serverb` với tư cách là người dùng student.

```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$
```
1.2 Xác minh rằng gói `tuned` đã được cài đặt.

```
[student@serverb ~]$ dnf list tuned
...output omitted...
Installed Packages
tuned.noarch              2.18.0-1.el9                 @System
```
1.3 Xác minh trạng thái dịch vụ `tuned` .

```
[student@serverb ~]$ systemctl is-active tuned
active
```
1.4 Liệt kê tất cả các tuning profiles khả dụng và mô tả của chúng. Lưu ý rằng cấu hình đang hoạt động hiện tại là `virtual-guest`.

```
[student@serverb ~]$ sudo tuned-adm list
[sudo] password for student: student
Available profiles:
- accelerator-performance   - Throughput performance based tuning with disabled
                              higher latency STOP states
- balanced                  - General non-specialized tuned profile
- desktop                   - Optimize for the desktop use-case
- hpc-compute               - Optimize for HPC compute workloads
- intel-sst                 - Configure for Intel Speed Select Base Frequency
- latency-performance       - Optimize for deterministic performance at the cost
                              of increased power consumption
- network-latency           - Optimize for deterministic performance at the cost
                              of increased power consumption, focused on low
                              latency network performance
- network-throughput        - Optimize for streaming network throughput, generally
                              only necessary on older CPUs or 40G+ networks
- optimize-serial-console   - Optimize for serial console use.
- powersave                 - Optimize for low power consumption
- throughput-performance    - Broadly applicable tuning that provides excellent
                              performance across a variety of common server
                              workloads
- virtual-guest             - Optimize for running inside a virtual guest
- virtual-host              - Optimize for running KVM guests
Current active profile: virtual-guest
```
1.5 Thay đổi  tuning profile đang hoạt động hiện tại thành `balanced` profile.

```
[student@serverb ~]$ sudo tuned-adm profile_info
Profile name:
balanced

Profile summary:
General non-specialized tuned profile
...output omitted...
```
1.6 Liệt kê thông tin tóm tắt của  tuned profile đang hoạt động hiện tại. Xác minh rằng cấu hình đang hoạt động là balanced profile.

```
[student@serverb ~]$ sudo tuned-adm profile_info
Profile name:
balanced

Profile summary:
General non-specialized tuned profile
...output omitted...
```

2. Hai tiến trình trên `serverb` đang chiếm dụng một tỷ lệ phần trăm CPU cao. Hãy điều chỉnh mức `nice` của mỗi tiến trình lên 10 để dành nhiều thời gian CPU hơn cho các tiến trình khác.

2.1 Xác định hai top tien trinh sử dụng nhiều CPU nhất trên máy chủ `serverb` . Lệnh ps liệt kê những tien trinh sử dụng nhiều CPU nhất ở cuối kết quả. Giá trị phần trăm CPU có thể khác nhau tùy theo máy của bạn.

```
[student@serverb ~]$ ps aux --sort=pcpu
USER    PID %CPU %MEM    VSZ   RSS TTY    STAT START   TIME COMMAND
...output omitted...
root    1079 98.5  0.1 225340  2300 ?     RN   06:25   4:29 sha1sum /dev/zero
root    1095 99.0  0.1 225340  2232 ?     R<   06:25   4:30 md5sum /dev/zero
```
2.2 Xác định mức độ `nice` hiện tại cho mỗi một trong hai tien trinh su dung CPU nhiều nhất.

```
[student@serverb ~]$ ps -o pid,pcpu,nice,comm \
$(pgrep sha1sum;pgrep md5sum)
  PID %CPU  NI COMMAND
 1079 98.8   2 sha1sum
 1095 99.1  -2 md5sum
```
2.3 Điều chỉnh mức `nice` cho mỗi quy trình thành 10. Sử dụng giá trị PID chính xác cho các quy trình của bạn từ đầu ra lệnh trước đó.

```
[student@serverb ~]$ sudo renice -n 10 1079 1095
[sudo] password for student: student
1079 (process ID) old priority 2, new priority 10
1095 (process ID) old priority -2, new priority 10
```
2.4 Xác minh rằng mức `nice` hiện tại cho mỗi tiến trình là 10.

```
[student@serverb ~]$ ps -o pid,pcpu,nice,comm \
$(pgrep sha1sum;pgrep md5sum)
  PID %CPU  NI COMMAND
 1079 98.9  10 sha1sum
 1095 99.2  10 md5sum
```
2.5 Return to the workstation machine as the student user.

```
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

---
# CHAPTER 6: Lab: Manage SELinux Security
Xác định các sự cố trong tệp nhật ký hệ thống và điều chỉnh cấu hình SELinux.   
Kết quả:  
- Xác định các sự cố trong tệp nhật ký hệ thống. - Điều chỉnh cấu hình SELinux.

1. Log in to the `serverb` machine as the `student` user and switch to the `root` user.
```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```
2. Từ trình duyệt web trên máy workstation , hãy truy cập trang web `http://serverb/lab.html`. Bạn sẽ thấy thông báo lỗi: `You do not have permission to access this resource.`

3. Nghiên cứu và xác định sự cố SELinux ngăn dịch vụ `Apache` cung cấp nội dung web.

3.1 Xem nội dung của tệp `/var/log/messages`. Sử dụng phím / và tìm kiếm chuỗi `sealert`. Sử dụng phím q để thoát lệnh less.

```
[root@serverb ~]# less /var/log/messages
...output omitted...
Apr  7 06:16:15 serverb setroubleshoot[26509]: failed to retrieve rpm info for /lab-content/la
b.html
Apr  7 06:16:17 serverb setroubleshoot[26509]: SELinux is preventing /usr/sbin/httpd from getattr access on the file /lab-content/lab.html. For complete SELinux messages run: sealert -l 35c9e452-2552-4ca3-8217-493b72ba6d0b
Apr  7 06:16:17 serverb setroubleshoot[26509]: SELinux is preventing /usr/sbin/httpd from getattr access on the file /lab-content/lab.html
...output omitted...
```

Note: Trong `less`, có thể tìm kiếm chuỗi bằng ký tự / giống như trong vi/vim:
- Gõ /chuoi_can_tim rồi nhấn Enter → `less` sẽ nhảy đến kết quả đầu tiên khớp.
- Gõ n → nhảy đến kết quả tiếp theo.
- Gõ N → nhảy đến kết quả trước đó.

3.2 Chạy lệnh `sealert` được đề xuất. Lưu ý ngữ cảnh nguồn, đối tượng đích, chính sách và chế độ thực thi.
```
[root@serverb ~]# sealert -l 35c9e452-2552-4ca3-8217-493b72ba6d0b
SELinux is preventing /usr/sbin/httpd from getattr access on the file /lab-content/lab.html.

*****  Plugin catchall_labels (83.8 confidence) suggests   *******************

If you want to allow httpd to have getattr access on the lab.html file
Then you need to change the label on /lab-content/lab.html
Do
# semanage fcontext -a -t FILE_TYPE '/lab-content/lab.html'
where FILE_TYPE is one of the following:
...output omitted...

Additional Information:
Source Context                system_u:system_r:httpd_t:s0
Target Context                unconfined_u:object_r:default_t:s0
Target Objects                /lab-content/lab.html [ file ]
Source                        httpd
Source Path                   /usr/sbin/httpd
Port                          <Unknown>
Host                          serverb.lab.example.com
Source RPM Packages           httpd-2.4.51-7.el9_0.x86_64
Target RPM Packages
SELinux Policy RPM            selinux-policy-targeted-34.1.27-1.el9.noarch
Local Policy RPM              selinux-policy-targeted-34.1.27-1.el9.noarch
Selinux Enabled               True
Policy Type                   targeted
Enforcing Mode                Enforcing
Host Name                     serverb.lab.example.com
Platform                      Linux serverb.lab.example.com
                              5.14.0-70.2.1.el9_0.x86_64 #1 SMP PREEMPT Wed Mar
                              16 18:15:38 EDT 2022 x86_64 x86_64
Alert Count                   8
First Seen                    2022-04-07 06:14:45 EDT
Last Seen                     2022-04-07 06:16:12 EDT
Local ID                      35c9e452-2552-4ca3-8217-493b72ba6d0b

Raw Audit Messages
type=AVC msg=audit(1649326572.86:407): avc:  denied  { getattr } for  pid=10731 comm="httpd" path="/lab-content/lab.html" dev="vda4" ino=18192752 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file permissive=0


type=SYSCALL msg=audit(1649326572.86:407): arch=x86_64 syscall=newfstatat success=no exit=EACCES a0=ffffff9c a1=7f7c8c0457c0 a2=7f7c887f7830 a3=100 items=0 ppid=10641 pid=10731 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm=httpd exe=/usr/sbin/httpd subj=system_u:system_r:httpd_t:s0 key=(null)

Hash: httpd,httpd_t,default_t,file,getattr
```

3.3 Phần *Raw Audit Messages* của lệnh `sealert` chứa thông tin từ tệp `/var/log/audit/audit.log`. Hãy tìm kiếm tệp `/var/log/audit/audit.log`. Tùy chọn `-m` tìm kiếm theo loại thông báo. Tùy chọn `ts` tìm kiếm theo thời gian. Mục sau đây xác định quy trình và tệp liên quan gây ra cảnh báo. Quy trình này là máy chủ web Apache httpd, tệp là `/lab-content/lab.html` và ngữ cảnh là `system_r:httpd_t`.
```
[root@serverb ~]# ausearch -m AVC -ts recent
...output omitted...
----
time->Thu Apr  7 06:16:12 2022
type=PROCTITLE msg=audit(1649326572.086:407): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
type=SYSCALL msg=audit(1649326572.086:407): arch=c000003e syscall=262 success=no exit=-13 a0=ffffff9c a1=7f7c8c0457c0 a2=7f7c887f7830 a3=100 items=0 ppid=10641 pid=10731 auid=4294967295 uid=48 gid=48 euid=48 suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
type=AVC msg=audit(1649326572.086:407): avc:  denied  { getattr } for  pid=10731 comm="httpd" path="/lab-content/lab.html" dev="vda4" ino=18192752 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file permissive=0
```

4. Hiển thị ngữ cảnh SELinux của thư mục tài liệu HTTP mới và thư mục tài liệu HTTP gốc. Giải quyết sự cố SELinux ngăn máy chủ Apache phục vụ nội dung web.  

4.1 So sánh ngữ cảnh SELinux cho các thư mục /lab-content và /var/www/html.

```
[root@serverb ~]# ls -dZ /lab-content /var/www/html
      unconfined_u:object_r:default_t:s0 /lab-content
system_u:object_r:httpd_sys_content_t:s0 /var/www/html
```
4.2 Tạo quy tắc ngữ cảnh tệp để đặt loại mặc định thành httpd_sys_content_ cho thư mục /lab-content và tất cả các tệp trong đó.
```
[root@serverb ~]# semanage fcontext -a \
-t httpd_sys_content_t '/lab-content(/.*)?'
```
4.3 Sửa ngữ cảnh SELinux cho các tệp trong thư mục `/lab-content`.
```
[root@serverb ~]# restorecon -R /lab-content/

```


5. Xác minh rằng máy chủ Apache hiện có thể phục vụ nội dung web.
Sử dụng trình duyệt web để làm mới liên kết `http://serverb/lab.html`. Nếu nội dung được hiển thị, thì sự cố của bạn đã được giải quyết.
```
This is the html file for the SELinux final lab on SERVERB.
```

6. Return to the workstation machine as the student user.  

```
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

---
# CHAPTER 7 : Lab: Manage Basic Storage
Tạo nhiều phân vùng trên một đĩa mới, định dạng một số phân vùng bằng hệ thống tệp và gắn kết chúng, đồng thời kích hoạt các phân vùng còn lại dưới dạng không gian hoán đổi.

Kết quả

Hiển thị và tạo phân vùng bằng lệnh parted.
- Tạo hệ thống tệp trên các phân vùng và gắn kết chúng liên tục.
- Tạo không gian hoán đổi và kích hoạt chúng khi khởi động.
- Với tư cách là người dùng học viên trên máy trạm, hãy sử dụng

1. Máy chủ ServerB có một số ổ đĩa chưa sử dụng. Trên ổ đĩa chưa sử dụng đầu tiên, hãy tạo nhãn phân vùng GPT và một phân vùng GPT 2 GB có tên là "backup".

Vì khó thiết lập kích thước chính xác, nên dung lượng từ 1,8 GB đến 2,2 GB là chấp nhận được.

Cấu hình phân vùng sao lưu để lưu trữ hệ thống tệp XFS.

1.1 Log in to serverb as the student user and switch to the root user.
```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```
1.2 Xác định các đĩa chưa sử dụng. Đĩa chưa sử dụng đầu tiên, `/dev/vdb`, không có phân vùng nào.

```
[root@serverb ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
vda    252:0    0   10G  0 disk
├─vda1 252:1    0    1M  0 part
├─vda2 252:2    0  200M  0 part /boot/efi
├─vda3 252:3    0  500M  0 part /boot
└─vda4 252:4    0  9.3G  0 part /
vdb    252:16   0    5G  0 disk
vdc    252:32   0    5G  0 disk
vdd    252:48   0    5G  0 disk
```
1.3 Xác nhận rằng đĩa `/dev/vdb` không có nhãn.

```
[root@serverb ~]# parted /dev/vdb print
Error: /dev/vdb: unrecognised disk label
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags:
```
1.4 Xác định sơ đồ phân vùng GPT.

```
[root@serverb ~]# parted /dev/vdb mklabel gpt
Information: You may need to update /etc/fstab.
```
1.5 Tạo phân vùng `backup` 2 GB với loại hệ thống tệp `xfs`. Bắt đầu phân vùng ở sector 2048.

```
[root@serverb ~]# parted /dev/vdb mkpart backup xfs 2048s 2GB
Information: You may need to update /etc/fstab.
```
1.6 Xác nhận việc tạo phân vùng `backup`.

```
[root@serverb ~]# parted /dev/vdb print
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name    Flags
 1      1049kB  2000MB  1999MB               backup
```
1.7 Chạy lệnh `udevadm settle`. Lệnh này chờ hệ thống phát hiện phân vùng mới và tạo tệp thiết bị `/dev/vdb1`.

```
[root@serverb ~]# udevadm settle
```

2. Định dạng phân vùng sao lưu 2 GB bằng hệ thống tệp XFS và gắn liên tục vào thư mục /﻿backup bằng cách sử dụng UUID.

2.1 Định dạng phân vùng `/dev/vbd1`.

```
[root@serverb ~]# mkfs.xfs /dev/vdb1
meta-data=/dev/vdb1              isize=512    agcount=4, agsize=121984 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1
data     =                       bsize=4096   blocks=487936, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```
2.2 Tạo điểm gắn kết `/backup`.

```
[root@serverb ~]# mkdir /backup

```
2.3 Trước khi thêm hệ thống tệp mới vào tệp `/etc/fstab`, hãy lấy UUID của tệp đó. UUID trên hệ thống của bạn có thể khác.

```
[root@serverb ~]# lsblk --fs /dev/vdb1
NAME FSTYPE FSVER LABEL UUID                                   FSAVAIL FSUSE% MOUNTPOINTS
vdb1 xfs                f74ed805-b1fc-401a-a5ee-140f97c6757d
```
2.4 Chỉnh sửa tệp /etc/fstab và xác định hệ thống tệp mới.

```
[root@serverb ~]# vim /etc/fstab
...output omitted...
UUID=f74ed805-b1fc-401a-a5ee-140f97c6757d   /backup   xfs   defaults   0 0
```
2.5 Buộc daemon systemd đọc lại tệp /etc/fstab.

```
[root@serverb ~]# systemctl daemon-reload

```
2.6 Gắn thủ công thư mục `/backup` để xác minh công việc của bạn. Xác nhận việc gắn kết thành công.

```
[root@serverb ~]# mount /backup
[root@serverb ~]# mount | grep /backup
/dev/vdb1 on /backup type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
```

3. Trên cùng một ổ đĩa, tạo hai phân vùng GPT 512 MB với tên là swap1 và swap2.  
Dung lượng từ 460 MB đến 564 MB là chấp nhận được.      
Cấu hình loại hệ thống tệp của các phân vùng để lưu trữ không gian hoán đổi.

3.1 Truy xuất vị trí kết thúc của phân vùng đầu tiên bằng cách hiển thị bảng phân vùng hiện tại trên đĩa `/dev/vdb`. Ở bước tiếp theo, bạn sử dụng giá trị đó làm điểm bắt đầu của phân vùng swap1.

```
[root@serverb ~]# parted /dev/vdb print
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name    Flags
 1      1049kB  2000MB  1999MB  xfs          backup
```
3.2 Tạo phân vùng GPT 512 MB đầu tiên có tên là swap1. Đặt loại của phân vùng này là linux-swap. Sử dụng vị trí cuối của phân vùng đầu tiên làm điểm bắt đầu. Vị trí cuối là 2000 MB + 512 MB = 2512 MB.

```
[root@serverb ~]# parted /dev/vdb mkpart swap1 linux-swap 2000M 2512M
Information: You may need to update /etc/fstab.
```
3.3 Tạo phân vùng GPT 512 MB thứ hai có tên là swap2. Đặt loại của phân vùng này là linux-swap. Sử dụng vị trí kết thúc của phân vùng trước làm điểm bắt đầu: 2512M. Vị trí kết thúc là 2512 MB + 512 MB = 3024 MB.

```
[root@serverb ~]# parted /dev/vdb mkpart swap2 linux-swap 2512M 3024M
Information: You may need to update /etc/fstab.
```
3.4 Hiển thị bảng phân vùng để xác minh công việc của bạn.

```
[root@serverb ~]# parted /dev/vdb print
Model: Virtio Block Device (virtblk)
Disk /dev/vdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name    Flags
 1      1049kB  2000MB  1999MB  xfs          backup
 2      2000MB  2512MB  513MB                swap1   swap
 3      2512MB  3024MB  512MB                swap2   swap
```
3.5 Chạy lệnh udevadm settle. Lệnh này sẽ chờ hệ thống đăng ký các phân vùng mới và tạo các tệp thiết bị.

```
[root@serverb ~]# udevadm settle

```



4. Khởi tạo hai phân vùng 512 MB làm không gian hoán đổi và cấu hình chúng để kích hoạt khi khởi động. Thiết lập không gian hoán đổi trên phân vùng swap2 được ưu tiên hơn phân vùng còn lại. Lưu ý rằng 512 MB tương đương với khoảng 488 MB. Hiển thị Giải pháp

4.1 Sử dụng lệnh mkswap để khởi tạo các phân vùng swap. Lưu ý mã UUID của hai không gian swap, vì bạn sẽ sử dụng thông tin đó ở bước tiếp theo. Nếu bạn xóa đầu ra mkswap, hãy sử dụng lệnh lsblk --fs để lấy mã UUID.
```
[root@serverb ~]# mkswap /dev/vdb2
Setting up swapspace version 1, size = 489 MiB (512749568 bytes)
no label, UUID=87976166-4697-47b7-86d1-73a02f0fc803
[root@serverb ~]# mkswap /dev/vdb3
Setting up swapspace version 1, size = 488 MiB (511700992 bytes)
no label, UUID=4d9b847b-98e0-4d4e-9ef7-dfaaf736b942
```
4.2 Chỉnh sửa tệp /etc/fstab và xác định không gian hoán đổi mới. Để đặt không gian hoán đổi trên phân vùng swap2 được ưu tiên hơn phân vùng swap1, hãy cấp cho phân vùng swap2 mức ưu tiên cao hơn bằng tùy chọn pri.

```
[root@serverb ~]# vim /etc/fstab
...output omitted...
UUID=a3665c6b-4bfb-49b6-a528-74e268b058dd   /backup xfs   defaults  0 0
UUID=87976166-4697-47b7-86d1-73a02f0fc803   swap    swap  pri=10    0 0
UUID=4d9b847b-98e0-4d4e-9ef7-dfaaf736b942   swap    swap  pri=20    0 0
```
4.3 Buộc daemon systemd đọc lại tệp /etc/fstab.

```
[root@serverb ~]# systemctl daemon-reload

```
4.4 Kích hoạt không gian hoán đổi mới. Xác minh việc kích hoạt không gian hoán đổi đã chính xác.

```
[root@serverb ~]# swapon -a
[root@serverb ~]# swapon --show
NAME      TYPE      SIZE USED PRIO
/dev/vdb2 partition 489M   0B   10
/dev/vdb3 partition 488M   0B   20
```


5. Để xác minh công việc của bạn, hãy khởi động lại máy `serverb`. Xác nhận rằng hệ thống tự động gắn phân vùng đầu tiên vào thư mục `/backup`. Đồng thời, hãy xác nhận rằng hệ thống đã kích hoạt hai swap spaces

5.1 Reboot serverb.
```
[root@serverb ~]# systemctl reboot
Connection to serverb closed by remote host.
Connection to serverb closed.
[student@workstation ~]$
```
5.2 Wait for serverb to boot, and then log in as the student user.
```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$
```
5.3 Xác minh rằng hệ thống tự động gắn phân vùng /dev/vdb1 vào thư mục /﻿backup.
```
[student@serverb ~]$ mount | grep /backup
/dev/vdb1 on /backup type xfs (rw,relatime,seclabel,attr2,inode64,noquota)
```
5.4 Xác minh rằng hệ thống kích hoạt cả hai swap spaces
```
[student@serverb ~]$ swapon --show
NAME      TYPE      SIZE USED PRIO
/dev/vdb2 partition 489M   0B   10
/dev/vdb3 partition 488M   0B   20
```
5.5 Return to the workstation machine as the student user.


```
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

---
# CHAPTER 10 : Lab: Control the Boot Process
Đặt lại mật khẩu root trên hệ thống, khôi phục sau khi cấu hình sai và đặt mục tiêu khởi động mặc định.   
Kết quả   
- Đặt lại mật khẩu bị mất cho người dùng root. 
- Chẩn đoán và khắc phục sự cố khởi động. 
- Đặt mục tiêu `systemd` mặc định.


1. 
1.12 Đặt `redhat` làm mật khẩu cho người dùng `root`.

```
sh-5.1# passwd root
Changing password for user root.
New password: redhat
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: redhat
passwd: all authentication tokens updated successfully.
```
1.13
```

```
1.14
```

```

3. Thay đổi mục tiêu systemd mặc định trên máy serverb để hệ thống tự động khởi động giao diện đồ họa khi khởi động.

Máy serverb không cài đặt giao diện đồ họa. Chỉ đặt mục tiêu mặc định và không cài đặt các gói.

3.1 Đăng nhập vào máy chủ với tư cách là người dùng `student` và chuyển sang người dùng root.


```
[student@workstation ~]$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```
3.2 Đặt graphical.target làm mục tiêu mặc định.
```
[root@serverb ~]# systemctl set-default graphical.target
Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/graphical.target.
```

3.3 Xác minh rằng mặc định đã được thiết lập chính xác.
```
[root@serverb ~]# systemctl get-default
graphical.target
```
3.4 Trở lại máy trạm với tư cách là người dùng là học viên.
```
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.
```

# CHAPTER 11 : Lab: Manage Network Security

Cấu hình tường lửa và cài đặt SELinux để cho phép truy cập vào nhiều máy chủ web chạy trên cùng một máy chủ.   
Kết quả   
- Cấu hình tường lửa và cài đặt SELinux trên máy chủ web.

1. Từ máy trạm, hãy kiểm tra quyền truy cập vào máy chủ web mặc định tại http://serverb.lab.example.com và máy chủ ảo http://serverb.lab.example.com:1001.

1.1 Kiểm tra quyền truy cập vào máy chủ web http://serverb.lab.example.com. Hiện tại, thử nghiệm không thành công. Máy chủ web sẽ trả về SERVER B.

```
[student@workstation ~]$ curl http://serverb.lab.example.com
curl: (7) Failed to connect to serverb.lab.example.com port 80: Connection refused
```
1.2 Kiểm tra quyền truy cập vào máy chủ ảo http://serverb.lab.example.com:1001. Kiểm tra hiện không thành công. Máy chủ ảo sẽ trả về VHOST 1.

```
[student@workstation ~]$ curl http://serverb.lab.example.com:1001
curl: (7) Failed to connect to serverb.lab.example.com port 1001: No route to host
```

2. Đăng nhập vào máy chủ để xác định nguyên nhân nào đang ngăn cản việc truy cập vào máy chủ web.

2.1
```

```
2.1
```

```
2.1
```

```
2.1
```

```
2.1
```

```

3. Cấu hình SELinux để cho phép dịch vụ httpd lắng nghe trên cổng 1001/TCP.

3.1 Sử dụng lệnh `semanage` để tìm đúng loại cổng.
```

```

3.2 Liên kết cổng 1001/TCP với kiểu http_port_t.
```

```

3.3
```

```
3.3
```

```
3.3
```

```
3.3
```

```

4. 

4.1
```

```
4.2
```

```

5. Đăng nhập vào máy chủ để xác định xem các cổng có được gán đúng cho tường lửa hay không.

5.1
```

```
5.1
```

```
5.1
```

```
5.1
```

```

6. Thêm cổng 1001/TCP vào cấu hình cố định cho vùng mạng công cộng. Xác nhận cấu hình của bạn.

6.1 Thêm cổng 1001/TCP vào vùng mạng công cộng.

```
[student@serverb ~]$ sudo firewall-cmd --permanent --zone=public \
--add-port=1001/tcp
success
```
6.2 Reload the firewall configuration.


```
[student@serverb ~]$ sudo firewall-cmd --reload
success
```
6.3 Verify your configuration.

```
[student@serverb ~]$ sudo firewall-cmd --zone=public --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: cockpit dhcpv6-client http ssh
  ports: 1001/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```
6.4 Return to the workstation machine as the student user.

```
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

7. Từ máy trạm, hãy xác nhận rằng máy chủ web mặc định tại http://serverb.lab.example.com trả về MÁY CHỦ B và máy chủ ảo tại http://serverb.lab.example.com:1001 trả về VHOST 1.

7.1 Test access to the http://serverb.lab.example.com web server.


```
[student@workstation ~]$ curl http://serverb.lab.example.com
SERVER B

```
7.2 Test access to the http://serverb.lab.example.com:1001 virtual host.
```
[student@workstation ~]$ curl http://serverb.lab.example.com:1001
VHOST 1
```