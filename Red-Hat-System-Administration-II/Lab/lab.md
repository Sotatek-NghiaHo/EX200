# CHAPTER 1 : Lab: Improve Command-line Productivity

Tạo một tập lệnh Bash có thể lọc và lấy thông tin liên quan từ các máy chủ khác nhau.

Kết quả
- Tạo một tập lệnh Bash và chuyển hướng đầu ra của nó vào một tệp.
- Sử dụng vòng lặp để đơn giản hóa mã của bạn.
- Lọc nội dung liên quan bằng cách sử dụng grep và biểu thức chính quy.

1. Tạo tệp lệnh thực thi `/home/student/bin/bash-lab` trên máy `workstation` . Nội dung ban đầu trong lệnh phải sử dụng chỉ thị thông dịch shebang.

Kiểm tra Bash shell nằm ở đâu bằng lệnh which hoặc type
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

![](../pic/41.png)

![](../pic/39.png)
![](../pic/40.png)   


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