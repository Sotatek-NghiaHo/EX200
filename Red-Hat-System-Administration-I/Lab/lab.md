# CHAPTER 1: Access the Command Line
- Chạy thành công các chương trình đơn giản từ dòng lệnh Bash shell.
- Thực hiện các lệnh để xác định loại tệp và hiển thị các phần của tệp văn bản.
- Thực hành sử dụng phím tắt lịch sử lệnh Bash để lặp lại lệnh hoặc một phần lệnh hiệu quả hơn.

1. Sử dụng lệnh để hiển thị thời gian và ngày hiện tại.
```
[root@redhat9-server-1 ~]# date
Wed Aug 13 08:24:16 AM +07 2025
``` 

2. Hiển thị thời gian hiện tại theo giờ 24 giờ (ví dụ: 13:57). Gợi ý: Chuỗi định dạng hiển thị đầu ra đó là %R.
```
[root@redhat9-server-1 ~]# date +%R
08:24
```

3. Tệp tin này là loại tệp gì /root/ ? Con người có thể đọc được không?

```
[root@redhat9-server-1 ~]# file script.sh 
script.sh: ASCII text
```
Tệp văn bản ASCII có thể đọc được bằng mắt thường

4. Sử dụng `wc` và phím tắt Bash để hiển thị kích thước của zcat tệp.
 wc để hiển thị số dòng, từ và byte
```
# trước đó
[root@redhat9-server-1 ~]# file script.sh 
script.sh: ASCII text
[root@redhat9-server-1 ~]# wc + Esc+.
[root@redhat9-server-1 ~]# wc script.sh
 6  3 13 script.sh
```
Trong đó:
- 6 → Số dòng trong file script.sh (line count).
- 3 → Số từ trong file script.sh (word count).
- 13 → Số byte trong file script.sh (byte count), tức là tổng số ký tự, bao gồm cả dấu xuống dòng và dấu cách.

5. Hiển thị 10 dòng đầu tiên của file.
```
[root@redhat9-server-1 ~]# head Esc+.
[root@redhat9-server-1 ~]# head script.sh 
sad

asd

a


```
Note: default head & tail display 10 line  
6. Hiển thị 10 dòng cuối cùng của file.
```
[root@redhat9-server-1 ~]# tail Esc+.
[root@redhat9-server-1 ~]# tail script.sh 
sad

asd

a
```
7. Lặp lại chính xác lệnh trước đó với bốn lần nhấn phím hoặc ít hơn.
- Một lựa chọn khác là nhấn phím Mũi tên Lên một lần để cuộn ngược lại một lệnh trong lịch sử lệnh, rồi nhấn Enter . Cách này sử dụng hai lần nhấn phím.
- Một phương pháp khác là nhập lệnh tắt !!rồi nhấn Enter để chạy lệnh gần đây nhất trong lịch sử lệnh
```
[root@redhat9-server-1 ~]# tail script.sh 
sad

asd

a

[root@redhat9-server-1 ~]# !!
tail script.sh 
sad

asd

a
```

8. Sử dụng tùy chọn `tail -n 20` để hiển thị 20 dòng cuối cùng trong file. Sử dụng tính năng chỉnh sửa dòng lệnh để thực hiện tác vụ này với số lần nhấn phím tối thiểu.

Sử dụng phím Mũi tên Lên để hiển thị lệnh trước đó. 
-> Tiếp theo, sử dụng tổ hợp phím Ctrl + A để di chuyển con trỏ đến đầu dòng. 
-> Sau đó, sử dụng tổ hợp phím Ctrl + Mũi tên Phải để nhảy đến từ tiếp theo. Thêm `-n 20` tùy chọn và nhấn Enter để thực thi lệnh.
```
[root@redhat9-server-1 ~]# tail -n 20 script.sh 
sad

asd

a
```

9. Sử dụng lịch sử shell để chạy `date +%R` lại lệnh.
```
[root@redhat9-server-1 ~]# history
  475  date +%R
  476  file zcat
  477  ls
  478  vi script.sh
[root@redhat9-server-1 ~]# !475
date +%R
08:49

```

# CHAPTER 3: Get Help from Local Documentation
(page 17/128)  
Trong hoạt động này, bạn tạo `my_task.txt` và sau đó thêm thông tin cụ thể vào tệp này cho từng bước.  
`touch my_task.txt`
1. Tìm kiếm và mở trang hướng dẫn sử dụng `hostname`. Tìm tùy chọn lệnh để hiển thị tất cả tên miền đủ điều kiện (FQDN) của máy. Sau đó, chạy `hostname` với tùy chọn in tất cả FQDN và gửi kết quả ra `my_task.txt`.

1.1 Tìm kiếm các trang hướng dẫn có chứa chuỗi "hostname"  
```
[root@redhat9-server-1 ~]# man -k hostname
freehostent (3)      - get network hostnames and addresses
gethostname (2)      - get/set hostname
gethostname (3p)     - get name of current host
getipnodebyaddr (3)  - get network hostnames and addresses
getipnodebyname (3)  - get network hostnames and addresses
hostname (1)         - show or set the system's host name
.....
```
1.2 Mở trang hướng dẫn của hostname và tìm kiếm tùy chọn hiển thị tất cả FQDN của máy.
```
[root@redhat9-server-1 ~]# man hostname

OPTIONS
       -a, --alias
              Display the alias name of the host (if used). This option is deprecated and should not be used anymore.

       -A, --all-fqdns
              Displays all FQDNs of the machine. This option enumerates all configured network addresses on all configured network  in‐
              terfaces,  and translates them to DNS domain names. Addresses that cannot be translated (i.e. because they do not have an
              appropriate reverse IP entry) are skipped. Note that different addresses may resolve to the same name, therefore the out‐
              put may contain duplicate entries. Do not make any assumptions about the order of the output.
```
"q" to quit the man page  
1.3 Sử dụng lệnh chuyển hướng với tùy chọn hostname -A để thêm tên máy chủ của máy vào my_task.txt
`[root@redhat9-server-1 ~]# hostname -A >> my_task.txt`  
1.4 Xác minh rằng tệp my_task.txt có chứa thông tin cần thiết.
```
[root@redhat9-server-1 ~]# cat my_task.txt 
Enforcing
redhat9-server-1 
```

2. Mở trang hướng dẫn của `date`. Tìm tùy chọn giúp bạn xác định số giây đã trôi qua giữa ngày 1 tháng 1 năm 1970 và ngày 1 tháng 1 năm 2025. Chạy lệnh và thêm kết quả đầu ra vào my_task.txt.

2.1 Duyệt trang hướng dẫn sử dụng date để tìm tùy chọn phù hợp.
```
       -d, --date=STRING
              display time described by STRING, not 'now'
       %s     seconds since 1970-01-01 00:00:00 UTC
EXAMPLES
       Convert seconds since the epoch (1970-01-01 UTC) to a date

              $ date --date='@2147483647'
```
"q" to quit the man page  
2.2 Sử dụng date với các tùy chọn -d và %s để lấy số giây giữa các ngày được yêu cầu. Thêm đầu ra của lệnh vào my_task.txt.
```
[root@redhat9-server-1 ~]# date -d "Jan 1 2025" +%s >> my_task.txt 
```

2.3 Xác minh rằng my_task.txt có chứa thông tin cần thiết.
```
[root@redhat9-server-1 ~]# cat my_task.txt 
Enforcing
redhat9-server-1 
1735664400
```

2.4 Bạn có thể xác minh kết quả bằng cách chuyển đổi số giây bạn lấy được thành ngày tháng.
```
[root@redhat9-server-1 ~]# date --date='@1735689600'
Wed Jan  1 07:00:00 AM +07 2025
```

3. Tìm trang hướng dẫn của lệnh xác định chế độ SELinux hiện tại của máy. Chạy lệnh và thêm kết quả vào my_task.txt.

3.1 Tìm lệnh hiển thị chế độ SELinux hiện tại.
```
[root@redhat9-server-1 ~]#  man -k selinux
getenforce (8)       - get the current mode of SELinux
```
3.2 Sử dụng getenforce để lấy chế độ SELinux hiện tại và thêm vào my_tasks.txt.
```
[root@redhat9-server-1 ~]# getenforce >> my_task.txt 

```
**3.3 Xác minh rằng my_task.txt có chứa thông tin cần thiết.**
```
[root@redhat9-server-1 ~]# cat my_task.txt 
Enforcing
redhat9-server-1 
1735664400
Enforcing
```
4. Mở trang hướng dẫn của man. Tìm thông tin về cách in trang hướng dẫn bằng PostScript. Nối lệnh, chứ không phải phần đầu ra, vào my_task.txt.    

**4.1 Sử dụng man man để xác định cách chuẩn bị trang hướng dẫn để in.**
```
[root@redhat9-server-1 ~]# man man
       man -t bash | lpr -Pps
           Format the manual page for bash into the default troff or groff format and pipe it to the printer  named  ps.   The  default
           output for groff is usually PostScript.  man --help should advise as to which processor is bound to the -t option.
```
**4.2 Sử dụng echo để thêm man có tùy chọn phù hợp vào my_tasks.txt.**
```
[root@redhat9-server-1 ~]# echo "man -t bash | lpr -Pps" >> my_task.txt
```
**4.3 Xác minh rằng my_task.txt có chứa thông tin cần thiết.**
```
[root@redhat9-server-1 ~]# cat my_task.txt 
Enforcing
redhat9-server-1 
1735664400
Enforcing
man -t bash | lpr -Pps
```
# CHAPTER 7: Manage Files from the Command Line   
7.7 page 36/128    
Sao chép, di chuyển, tạo, xóa và sắp xếp các tệp từ dòng lệnh.   
Target: Sử dụng ký tự đại diện để định vị và thao tác với tệp.

2. Tạo một thư mục có tên là `project_plans` trong thu muc Documents . Thư muc Documents này được đặt trong thư mục gốc của người dùng `nghiahv`. Tạo hai tệp trống trong project_plans có tên là `season1_project_plan.odf` và `season2_project_plan.odf`.  

Gợi ý: Nếu thư mục ~/Documents không tồn tại, hãy sử dụng tùy chọn lệnh mkdir -p để tạo thư mục.
```
[nghiahv@redhat9-server-1 ~]$ mkdir -p Documents/project_plans
[nghiahv@redhat9-server-1 ~]$ touch \
> Documents/project_plans/{season1,season2}_project_plan.odf

[nghiahv@redhat9-server-1 ~]$ ls -lR Documents/
Documents/:
total 0
drwxr-xr-x. 2 nghiahv nghiahv 70 Aug 13 10:21 project_plans

Documents/project_plans:
total 0
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:21 season1_project_plan.odf
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:21 season2_project_plan.odf
```
3. Tạo 12 tệp với tên `tv_seasonX_episodeY.ogg` trong thư mục /home/student. Thay thế ký tự X bằng số mùa và ký tự Y bằng tập của mùa đó, cho hai mùa, mỗi mùa sáu tập.
```
[nghiahv@redhat9-server-1 ~]$ touch tv_season{1..2}_episode{1..6}.ogg
[nghiahv@redhat9-server-1 ~]$ ls tv*
tv_season1_episode1.ogg  tv_season1_episode4.ogg  tv_season2_episode1.ogg  tv_season2_episode4.ogg
tv_season1_episode2.ogg  tv_season1_episode5.ogg  tv_season2_episode2.ogg  tv_season2_episode5.ogg
tv_season1_episode3.ogg  tv_season1_episode6.ogg  tv_season2_episode3.ogg  tv_season2_episode6.ogg

```

4. Là tác giả của một loạt tiểu thuyết trinh thám thành công, bạn đang biên tập các chương của cuốn sách bán chạy tiếp theo để xuất bản. Hãy tạo tám tệp với tên `mystery_chapterX.odf`. Thay thế ký tự X bằng các số từ 1 đến 8.
```
[nghiahv@redhat9-server-1 ~]$ touch mystery_chapter{1..8}.odf
[nghiahv@redhat9-server-1 ~]$ ls mys*
mystery_chapter1.odf  mystery_chapter3.odf  mystery_chapter5.odf  mystery_chapter7.odf
mystery_chapter2.odf  mystery_chapter4.odf  mystery_chapter6.odf  mystery_chapter8.odf

```
5. Sử dụng một lệnh duy nhất để tạo thư mục `~/Videos/season1` và `~/Videos/season2` để sắp xếp các tập phim truyền hình. Di chuyển các tập phim truyền hình phù hợp vào các thư mục con của mùa phim. Chỉ sử dụng hai lệnh và chỉ định đích đến bằng cú pháp tương ứng.  

Tạo hai thư mục con có tên là season1 và season2 trong thư mục Videos bằng cách sử dụng một lệnh duy nhất.
```
[nghiahv@redhat9-server-1 ~]$ mkdir -p Videos/season{1..2}
[nghiahv@redhat9-server-1 ~]$ ls Videos/
season1  season2
```
previous result
```
[nghiahv@redhat9-server-1 ~]$ ls tv*
tv_season1_episode1.ogg  tv_season1_episode4.ogg  tv_season2_episode1.ogg  tv_season2_episode4.ogg
tv_season1_episode2.ogg  tv_season1_episode5.ogg  tv_season2_episode2.ogg  tv_season2_episode5.ogg
tv_season1_episode3.ogg  tv_season1_episode6.ogg  tv_season2_episode3.ogg  tv_season2_episode6.ogg

```
Di chuyển các tập phim truyền hình phù hợp vào thư mục con theo mùa chỉ bằng hai lệnh.
```
[nghiahv@redhat9-server-1 ~]$ mv tv_season1* Videos/season1/
[nghiahv@redhat9-server-1 ~]$ mv tv_season2* Videos/season2/
[nghiahv@redhat9-server-1 ~]$ ls -R Videos/
Videos/:
season1  season2

Videos/season1:
tv_season1_episode1.ogg  tv_season1_episode3.ogg  tv_season1_episode5.ogg
tv_season1_episode2.ogg  tv_season1_episode4.ogg  tv_season1_episode6.ogg

Videos/season2:
tv_season2_episode1.ogg  tv_season2_episode3.ogg  tv_season2_episode5.ogg
tv_season2_episode2.ogg  tv_season2_episode4.ogg  tv_season2_episode6.ogg
```
6. Tạo một hệ thống phân cấp thư mục hai cấp chỉ bằng một lệnh để sắp xếp các chương sách bí ẩn. Tạo thư mục con `my_bestseller` trong thư mục `Documents`, và tạo thư mục con `chapters` trong thư mục `my_bestseller` mới. Tạo thêm ba thư mục con ngay trong thư mục `my_bestseller` chỉ bằng một lệnh. Đặt tên cho các thư mục con này là `editor, changes và vacation`. Bạn không cần sử dụng lệnh mkdir -p để tạo thư mục cha vì thư mục cha my_bestseller đã tồn tại.

6.1 Tạo thư mục my_bestseller trong thư mục Documents. Tạo thư mục chapters trong thư mục my_bestseller.
```
[nghiahv@redhat9-server-1 ~]$ mkdir -p Documents/my_bestseller/chapters
[nghiahv@redhat9-server-1 ~]$ ls -R Documents/
Documents/:
my_bestseller  project_plans

Documents/my_bestseller:
chapters

Documents/my_bestseller/chapters:

Documents/project_plans:
season1_project_plan.odf  season2_project_plan.odf

```

6.2 Tạo ba thư mục có tên là `editor`, `changes` và `vacation` trong thư mục my_bestseller bằng cách sử dụng một lệnh duy nhất.
```
[nghiahv@redhat9-server-1 ~]$ mkdir Documents/my_bestseller/{editor,changes,vacation}
[nghiahv@redhat9-server-1 ~]$ ls -R Documents/
Documents/:
my_bestseller  project_plans

Documents/my_bestseller:
changes  chapters  editor  vacation

Documents/my_bestseller/changes:

Documents/my_bestseller/chapters:

Documents/my_bestseller/editor:

Documents/my_bestseller/vacation:

Documents/project_plans:
season1_project_plan.odf  season2_project_plan.odf

```
7. Chuyển đến thư mục `chapters`. Sử dụng phím tắt thư mục home dấu ngã (~) để di chuyển tất cả các chương sách đến thư mục `chapters`, hiện là thư mục hiện tại của bạn. Sử dụng cú pháp đơn giản nhất để chỉ định thư mục đích.

Bạn muốn gửi hai chương đầu tiên đến trình soạn thảo để xem xét. Chỉ di chuyển hai chương đó đến thư mục trình soạn thảo để tránh sửa đổi chúng trong quá trình xem xét. Bắt đầu từ thư mục con chương, sử dụng dấu ngoặc nhọn mở rộng với một phạm vi để chỉ định tên tệp chương cần di chuyển và đường dẫn tương đối đến thư mục đích.

Trong thời gian nghỉ, bạn dự định viết chương 7 và 8. Sử dụng một lệnh duy nhất để di chuyển các tệp từ thư mục chương sang thư mục nghỉ. Chỉ định tên tệp chương bằng cách sử dụng dấu ngoặc nhọn mở rộng với danh sách các chuỗi và không sử dụng ký tự đại diện.

7.1 Chuyển đến thư mục `chapters` và sử dụng phím tắt thư mục gốc dấu ngã (~) để di chuyển tất cả các chương sách vào thư mục chương.
```
[nghiahv@redhat9-server-1 ~]$ cd Documents/my_bestseller/chapters/
[nghiahv@redhat9-server-1 chapters]$ mv ~/mystery_chapter* .
[nghiahv@redhat9-server-1 chapters]$ ls
mystery_chapter1.odf  mystery_chapter3.odf  mystery_chapter5.odf  mystery_chapter7.odf
mystery_chapter2.odf  mystery_chapter4.odf  mystery_chapter6.odf  mystery_chapter8.odf

```

7.2 Di chuyển hai chương đầu tiên đến thư mục `editor`. Sử dụng dấu ngoặc nhọn mở rộng với phạm vi để chỉ định tên tệp chương cần di chuyển và đường dẫn tương đối đến thư mục đích.

```
[nghiahv@redhat9-server-1 chapters]$ mv mystery_chapter{1..2}.odf ../editor/
[nghiahv@redhat9-server-1 chapters]$ ls ../editor/
mystery_chapter1.odf  mystery_chapter2.odf
[nghiahv@redhat9-server-1 chapters]$ ls
mystery_chapter3.odf  mystery_chapter4.odf  mystery_chapter5.odf  mystery_chapter6.odf  mystery_chapter7.odf  mystery_chapter8.odf

```
```
[nghiahv@redhat9-server-1 chapters]$ pwd
/home/nghiahv/Documents/my_bestseller/chapters

[nghiahv@redhat9-server-1 chapters]$ tree -d -L 2 /home/nghiahv/Documents/
/home/nghiahv/Documents/
├── my_bestseller
│   ├── changes
│   ├── chapters
│   ├── editor
│   └── vacation
└── project_plans

6 directories
```
Command cd

Lệnh	|Mô tả
---|---
cd	|Về thư mục home hoặc đến thư mục chỉ định
cd -	|Quay lại thư mục trước đó
cd ..	|Lùi một cấp thư mục cha

7.3 Sử dụng một lệnh duy nhất để di chuyển chương 7 và 8 từ thư mục `chapters` sang thư mục `vacation`. Chỉ định tên tệp chương bằng cách sử dụng dấu ngoặc nhọn mở rộng với danh sách chuỗi và không sử dụng ký tự đại diện.
```
[nghiahv@redhat9-server-1 chapters]$ mv mystery_chapter{7..8}.odf ../vacation/
[nghiahv@redhat9-server-1 chapters]$ ls
mystery_chapter3.odf  mystery_chapter4.odf  mystery_chapter5.odf  mystery_chapter6.odf
[nghiahv@redhat9-server-1 chapters]$ ls ../vacation/
mystery_chapter7.odf  mystery_chapter8.odf
```
8. Chuyển thư mục làm việc của bạn thành `~/Videos/season2`, sau đó sao chép tập đầu tiên của mùa phim vào thư mục `vacation`. Sử dụng một lệnh cd duy nhất để chuyển từ thư mục làm việc sang thư mục `~/Documents/my_bestseller/vacation`. Liệt kê các tệp của thư mục đó. Sử dụng đối số thư mục làm việc trước đó để quay lại thư mục `season2`. Đối số này thành công nếu lần thay đổi thư mục cuối cùng bằng lệnh cd chỉ sử dụng một lệnh thay vì nhiều lệnh cd. Từ thư mục `season2`, sao chép tệp tập 2 vào thư mục `vacation`. Sử dụng phím tắt một lần nữa để quay lại thư mục `vacation`.  

8.1 Thay đổi thư mục làm việc của bạn thành `~/Videos/season2`, sau đó sao chép tập đầu tiên của mùa giải vào thư mục `vacation`.
```
[nghiahv@redhat9-server-1 chapters]$ cd ~/Videos/season2/
[nghiahv@redhat9-server-1 season2]$ ls
tv_season2_episode1.ogg  tv_season2_episode3.ogg  tv_season2_episode5.ogg
tv_season2_episode2.ogg  tv_season2_episode4.ogg  tv_season2_episode6.ogg
[nghiahv@redhat9-server-1 season2]$ cp *episode1.ogg ~/Documents/my_bestseller/vacation/
[nghiahv@redhat9-server-1 season2]$ ls ~/Documents/my_bestseller/vacation/
mystery_chapter7.odf  mystery_chapter8.odf  tv_season2_episode1.ogg

```
8.2 Sử dụng một lệnh cd duy nhất để chuyển từ thư mục làm việc sang thư mục `~/Documents/my_bestseller/vacation`, liệt kê các tệp trong đó và sử dụng tham số - để trở về thư mục trước đó. Sao chép tệp tập 2 vào thư mục `vacation`. Sử dụng lệnh cd với tham số - để trở về thư mục `vacation`.
```
[nghiahv@redhat9-server-1 season2]$ cd ~/Documents/my_bestseller/vacation/
[nghiahv@redhat9-server-1 vacation]$ ls
mystery_chapter7.odf  mystery_chapter8.odf  tv_season2_episode1.ogg
[nghiahv@redhat9-server-1 vacation]$ cd -
/home/nghiahv/Videos/season2
[nghiahv@redhat9-server-1 season2]$ cp *episode2.ogg ~/Documents/my_bestseller/vacation/
[nghiahv@redhat9-server-1 season2]$ cd -
/home/nghiahv/Documents/my_bestseller/vacation
[nghiahv@redhat9-server-1 vacation]$ ls
mystery_chapter7.odf  mystery_chapter8.odf  tv_season2_episode1.ogg  tv_season2_episode2.ogg
```

9. Tác giả của chương 5 và 6 muốn thử nghiệm các thay đổi có thể. Hãy sao chép cả hai tệp từ thư mục `~/Documents/my_bestseller/chapters` sang thư mục `~/Documents/my_bestseller/changes` để ngăn những thay đổi này làm thay đổi các tệp gốc. 

Truy cập thư mục `~/Documents/my_bestseller`. Sử dụng mẫu so khớp trong ngoặc vuông để chỉ định số chương nào sẽ khớp trong đối số tên tệp của lệnh cp.
```
[nghiahv@redhat9-server-1 vacation]$ cd ~/Documents/my_bestseller/
[nghiahv@redhat9-server-1 my_bestseller]$ ls
changes  chapters  editor  vacation
[nghiahv@redhat9-server-1 my_bestseller]$ cp chapters/mystery_chapter[56].odf changes/
[nghiahv@redhat9-server-1 my_bestseller]$ ls chapters/
mystery_chapter3.odf  mystery_chapter4.odf  mystery_chapter5.odf  mystery_chapter6.odf
[nghiahv@redhat9-server-1 my_bestseller]$ ls changes/
mystery_chapter5.odf  mystery_chapter6.odf
```

10. Chuyển thư mục hiện tại của bạn thành thư mục `changes` và sử dụng lệnh `date +%F` kết hợp với lệnh copy để sao chép tệp `mystery_chapter5.odf` sang một tệp mới chứa ngày tháng đầy đủ. Sử dụng định dạng tên `mystery_chapter5_YYYY-MM-DD.odf`.

Bằng cách sử dụng lệnh thay thế với lệnh` date +%s`, hãy tạo một bản sao khác của tệp `mystery_chapter5.odf` và thêm dấu thời gian hiện tại (là số giây kể từ thời điểm epoch, 1970-01-01 00:00 UTC) để đảm bảo tên tệp duy nhất.
```
[nghiahv@redhat9-server-1 changes]$ cp mystery_chapter5.odf \
> mystery_chapter5_$(date +%F).odf
[nghiahv@redhat9-server-1 changes]$ cp mystery_chapter5.odf mystery_chapter5_$(date +%s).odf
[nghiahv@redhat9-server-1 changes]$ ls
mystery_chapter5_1755058581.odf  mystery_chapter5_2025-08-13.odf  mystery_chapter5.odf  mystery_chapter6.odf

```
Note: Phần $(...) là command substitution (thay thế bằng kết quả của lệnh bên trong).

11. Sau khi xem xét kỹ hơn, bạn quyết định rằng mình không cần thay đổi cốt truyện. Hãy xóa thư mục `changes`.

Nếu cần, hãy vào thư mục `changes` và xóa tất cả các tệp trong thư mục đó. Bạn không thể xóa một thư mục khi nó là thư mục làm việc hiện tại.

Chuyển đến thư mục cha của thư mục `changes`. Hãy thử xóa thư mục trống bằng lệnh rm mà không có tùy chọn -r đệ quy. Thử này sẽ thất bại. Cuối cùng, hãy sử dụng lệnh `rmdir` để xóa thư mục trống, và thành công.

Khi kỳ nghỉ kết thúc, bạn không cần thư mục `vacation` nữa. Hãy xóa nó bằng lệnh rm với tùy chọn đệ quy.

Khi hoàn tất, hãy quay lại thư mục home của người dùng nghiahv.
```
[nghiahv@redhat9-server-1 changes]$ ls
mystery_chapter5_1755058581.odf  mystery_chapter5_2025-08-13.odf  mystery_chapter5.odf  mystery_chapter6.odf
[nghiahv@redhat9-server-1 changes]$ rm mystery_chapter*
[nghiahv@redhat9-server-1 changes]$ ls
[nghiahv@redhat9-server-1 changes]$ cd ..
[nghiahv@redhat9-server-1 my_bestseller]$ ls
changes  chapters  editor  vacation
[nghiahv@redhat9-server-1 my_bestseller]$ rm changes/
rm: cannot remove 'changes/': Is a directory
[nghiahv@redhat9-server-1 my_bestseller]$ rmdir changes/
[nghiahv@redhat9-server-1 my_bestseller]$ ls
chapters  editor  vacation
[nghiahv@redhat9-server-1 my_bestseller]$ rm -r vacation/
[nghiahv@redhat9-server-1 my_bestseller]$ ls
chapters  editor
[nghiahv@redhat9-server-1 my_bestseller]$ cd
[nghiahv@redhat9-server-1 ~]$ 

```

12. Tạo một liên kết cứng đến tệp `~/Documents/project_plans/season2_project_plan.odf` có tên là `~/Documents/backups/season2_project_plan.odf`.back. Liên kết cứng giúp bảo vệ tệp gốc khỏi việc vô tình xóa nhầm và giữ cho tệp sao lưu được cập nhật khi bạn thay đổi tệp gốc.

Gợi ý: Nếu thư mục `~/Documents/backups` không tồn tại, hãy sử dụng lệnh mkdir để tạo thư mục đó.
```
[nghiahv@redhat9-server-1 ~]$ mkdir ~/Documents/backups
[nghiahv@redhat9-server-1 ~]$ ln Documents/project_plans/season2_project_plan.odf  \
> Documents/backups/season2_project_plan.odf.back
[nghiahv@redhat9-server-1 ~]$ ls -lR Documents/
Documents/:
total 0
drwxr-xr-x. 2 nghiahv nghiahv 43 Aug 13 13:43 backups
drwxr-xr-x. 4 nghiahv nghiahv 36 Aug 13 13:40 my_bestseller
drwxr-xr-x. 2 nghiahv nghiahv 70 Aug 13 10:21 project_plans

Documents/backups:
total 0
-rw-r--r--. 2 nghiahv nghiahv 0 Aug 13 10:21 season2_project_plan.odf.back

Documents/my_bestseller:
total 0
drwxr-xr-x. 2 nghiahv nghiahv 118 Aug 13 10:55 chapters
drwxr-xr-x. 2 nghiahv nghiahv  62 Aug 13 10:46 editor

Documents/my_bestseller/chapters:
total 0
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:28 mystery_chapter3.odf
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:28 mystery_chapter4.odf
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:28 mystery_chapter5.odf
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:28 mystery_chapter6.odf

Documents/my_bestseller/editor:
total 0
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:28 mystery_chapter1.odf
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:28 mystery_chapter2.odf

Documents/project_plans:
total 0
-rw-r--r--. 1 nghiahv nghiahv 0 Aug 13 10:21 season1_project_plan.odf
-rw-r--r--. 2 nghiahv nghiahv 0 Aug 13 10:21 season2_project_plan.odf

```
# CHAPTER 10: Manage Local Users and Groups
10.11 - PAGE 55/128
Sử dụng quyền truy cập superuser để quản lý người dùng và nhóm cục bộ và để quản lý chính sách mật khẩu cục bộ.

Kết quả:
- Đặt chính sách thời hạn mật khẩu mặc định cho mật khẩu người dùng cục bộ.
- Tạo và sử dụng nhóm bổ sung cho người dùng mới.
- Tạo ba người dùng với nhóm bổ sung mới.
- Đặt mật khẩu ban đầu cho người dùng đã tạo.
- Cấu hình các thành viên nhóm bổ sung để sử dụng sudo để chạy bất kỳ lệnh nào với tư cách là bất kỳ người dùng nào.
- Đặt chính sách thời hạn mật khẩu dành riêng cho người dùng.

Chuyển sang người dùng `sudo -i `   
2. đảm bảo rằng người dùng mới tạo phải thay đổi mật khẩu sau mỗi 30 ngày.  (KHÔNG áp dụng cho user hiện có.)
```
vi /etc/login.defs
PASS_MAX_DAYS   30
```
3. Tạo nhóm `consultants` có GID là 35000.
```
[root@redhat9-server-1 ~]#  groupadd -g 35000 consultants
[root@redhat9-server-1 ~]# cat /etc/group | grep consultants
consultants:x:35000:

```
4.  Cấu hình quyền quản trị để cho phép tất cả thành viên nhóm `consultants` thực thi bất kỳ lệnh nào với tư cách người dùng. Tránh sử dụng công cụ dòng lệnh visudo để chỉnh sửa tệp /etc/sudoers. Thay vào đó, hãy đặt tệp cấu hình vào thư mục `/etc/sudoers.d`.
```
vi /etc/sudoers.d/consultants
%consultants  ALL=(ALL) ALL
```
Note:
- dấu % ở đầu nghĩa là nhóm (group) trong Linux.
- Không có dấu % → nghĩa là tài khoản người dùng cụ thể.
5. Tạo người dùng `consultant1`, `consultant2` và `consultant3` với nhóm `consultants` là nhóm bổ sung của họ.
```
[root@redhat9-server-1 ~]# useradd -G consultants consultant1
[root@redhat9-server-1 ~]# useradd -G consultants consultant2
[root@redhat9-server-1 ~]# useradd -G consultants consultant3
[root@redhat9-server-1 ~]# tail /etc/passwd
nghiahv:x:1000:1000:Nghiahv:/home/nghiahv:/bin/bash
sysadmin1:x:1001:1002::/home/sysadmin1:/bin/bash
consultant1:x:1002:35001::/home/consultant1:/bin/bash
consultant2:x:1003:1003::/home/consultant2:/bin/bash
consultant3:x:1004:1004::/home/consultant3:/bin/bash

[root@redhat9-server-1 ~]# id consultant2
uid=1003(consultant2) gid=1003(consultant2) groups=1003(consultant2),35000(consultants)
[root@redhat9-server-1 ~]# id consultant3
uid=1004(consultant3) gid=1004(consultant3) groups=1004(consultant3),35000(consultants)
[root@redhat9-server-1 ~]# id consultant1
uid=1002(consultant1) gid=35001(consultant1) groups=35001(consultant1),35000(consultants)

```
Note:
- tuy chon `-G` : chỉ thêm user vào nhóm phụ consultants, không đặt nhóm chính (primary group) là consultants
- Muốn thêm user vào nhóm bổ sung(supplementary groups):  
`usermod -aG groupname username`
- Đổi nhóm chính:  
`sudo usermod -g new_primary_group username`
6. Đặt mật khẩu consultant1, consultant2 và consultant3 la redhat.
```
[root@redhat9-server-1 ~]# passwd consultant1
Changing password for user consultant1.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.

[root@redhat9-server-1 ~]# passwd consultant2
Changing password for user consultant2.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.

[root@redhat9-server-1 ~]# passwd consultant3
Changing password for user consultant3.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.


```
7. Đặt thời hạn hết hạn cho các tài khoản consultant1, consultant2 và consultant3 là 90 ngày kể từ ngày hiện tại.

7.1 Xác định ngày trong tương lai 90 ngày. 

```
[root@redhat9-server-1 ~]# date -d "+90 days" +%F
2025-11-11

```
7.2 Đặt các tài khoản consultant1, consultant2, và consultant3 hết hạn vào ngày được hiển thị ở bước trước
```
[root@redhat9-server-1 ~]# chage -E 2025-11-11 consultant1 
[root@redhat9-server-1 ~]# chage -E 2025-11-11 consultant2
[root@redhat9-server-1 ~]# chage -E 2025-11-11 consultant3

```

8. Thay đổi chính sách mật khẩu cho tài khoản consultant2 để yêu cầu mật khẩu mới sau mỗi 15 ngày.

```
[root@redhat9-server-1 ~]# chage -M 15 consultant2


```
9. Ngoài ra, hãy buộc người dùng consultant1, consultant2 và consultant3 thay đổi mật khẩu khi đăng nhập lần đầu.

Đặt ngày cuối cùng thay đổi mật khẩu thành 0 để người dùng phải thay đổi mật khẩu khi lần đầu đăng nhập vào hệ thống.


```
[root@redhat9-server-1 ~]# chage -d 0 consultant1
[root@redhat9-server-1 ~]# chage -d 0 consultant2
[root@redhat9-server-1 ~]# chage -d 0 consultant3
```

Ket qua
```
[root@redhat9-server-1 ~]# chage -l consultant1
Last password change					: password must be changed
Password expires					: password must be changed
Password inactive					: password must be changed
Account expires						: Nov 11, 2025
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
[root@redhat9-server-1 ~]# chage -l consultant2
Last password change					: password must be changed
Password expires					: password must be changed
Password inactive					: password must be changed
Account expires						: Nov 11, 2025
Minimum number of days between password change		: 0
Maximum number of days between password change		: 15
Number of days of warning before password expires	: 7
[root@redhat9-server-1 ~]# chage -l consultant3
Last password change					: password must be changed
Password expires					: password must be changed
Password inactive					: password must be changed
Account expires						: Nov 11, 2025
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
```

---
# CHAPTER 11: Control Access to Files
11.7 - page 63/128  
Đặt quyền tiêu chuẩn cho các tệp và giải thích tác động bảo mật của các cài đặt quyền khác nhau.

Kết quả:
- Tạo một thư mục nơi người dùng có thể cùng nhau làm việc trên các tập tin.
- Tạo cấu trúc tệp và thư mục cần thiết, chỉ định quyền truy cập theo yêu cầu.  

Chuyển sang người dùng `sudo -i `     
2. Tạo thư mục cộng tác `techdocs` trong thư mục `/home`. Đặt quyền sở hữu nhóm của thư mục thành nhóm `techdocs`, cấp toàn quyền cho người dùng và nhóm, và cấu hình thư mục sao cho chỉ chủ sở hữu tệp mới có thể xóa tệp của họ.  

Ban dau
```
# Tạo group
groupadd techdocs

# Tạo user và gán group
useradd -g techdocs dev1
useradd -g techdocs dev2
useradd -g techdocs editor1

# Tạo user không thuộc nhóm techdocs
useradd dbadmin1

```

2.1 Tạo thư mục /home/techdocs.
```
mkdir /home/techdocs
[root@redhat9-server-1 ~]# ll /home/
drwxr-xr-x.  2 root        root           6 Aug 13 14:46 techdocs

```
2.2 Thay đổi quyền sở hữu nhóm cho thư mục /home/techdocs thành nhóm techdocs.
```
[root@redhat9-server-1 ~]# chown :techdocs /home/techdocs/
[root@redhat9-server-1 ~]# ll /home/
drwxr-xr-x.  2 root        techdocs       6 Aug 13 14:46 techdocs
```
2.3 Đặt quyền đọc, ghi và thực thi cho người dùng và nhóm, không cấp quyền cho những người khác trong thư mục /home/techdocs.
```
[root@redhat9-server-1 ~]# chmod 0770 /home/techdocs
[root@redhat9-server-1 ~]# ll /home/
drwxrwx---.  2 root        techdocs       6 Aug 13 14:46 techdocs

```
2.4 Gán bit cố định vào thư mục /home/techdocs.

```
[root@redhat9-server-1 ~]#  chmod o+t /home/techdocs
[root@redhat9-server-1 ~]# ll /home/
drwxrwx--T.  2 root        techdocs       6 Aug 13 14:46 techdocs
```

2.5 Liệt kê các quyền của thư mục.
```
[root@redhat9-server-1 ~]# ls -ld /home/techdocs
drwxrwx--T. 2 root techdocs 6 Aug 13 14:46 /home/techdocs
# Bộ quyền --T cho thấy thư mục được cấu hình với quyền sticky bit
```

3. Xác minh rằng người dùng trong nhóm `techdocs` có thể tạo và cộng tác trên các tệp trong thư mục `/home/techdocs`. Với tư cách là người dùng dev1, hãy tạo tệp `techdoc1.txt` trong thư mục `/home/techdocs`. Với tư cách là người dùng dev2, hãy thêm nội dung sau vào tệp: "This is the first tech doc". Cấu hình quyền sở hữu và quyền hạn nhóm cần thiết để cho phép cộng tác này.

3.1 Chuyển sang người dùng dev1. Chuyển đến thư mục `/home/techdocs`.
```
[root@redhat9-server-1 ~]# su - dev1
[dev1@redhat9-server-1 ~]$ cd /home/techdocs/
```
3.2 Tạo tệp `techdoc1.txt` trong thư mục `/home/techdocs`.
```
[dev1@redhat9-server-1 techdocs]$ touch techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ ls
techdoc1.txt
```
3.3 Thay đổi quyền sở hữu nhóm cho tệp `techdoc1.txt` thành nhóm techdocs. Thêm quyền ghi cho nhóm trên tệp `techdoc1.txt`. Liệt kê các quyền của tệp.

```
[dev1@redhat9-server-1 techdocs]$ ll
total 0
-rw-r--r--. 1 dev1 techdocs 0 Aug 13 15:11 techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ chown :techdocs techdoc1.txt 
[dev1@redhat9-server-1 techdocs]$ ll
total 0
-rw-r--r--. 1 dev1 techdocs 0 Aug 13 15:11 techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ chmod g+w techdoc1.txt 
[dev1@redhat9-server-1 techdocs]$ ll
total 0
-rw-rw-r--. 1 dev1 techdocs 0 Aug 13 15:11 techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ 

```
3.4 Exit from the dev1 user shell. Switch to the dev2 user.
```
[dev1@redhat9-server-1 techdocs]$ exit
logout
[root@redhat9-server-1 ~]# su - dev2
[dev2@redhat9-server-1 ~]$ 

```
2 user dev1 & dev2
```
[dev2@redhat9-server-1 ~]$ id dev2
uid=1006(dev2) gid=35002(techdocs) groups=35002(techdocs)
[dev2@redhat9-server-1 ~]$ id dev1
uid=1005(dev1) gid=35002(techdocs) groups=35002(techdocs)
```

3.5 Chuyển đến thư mục `/home/techdocs`. Xác minh rằng người dùng dev2 có thể ghi vào tệp techdoc1.txt.
```
[dev2@redhat9-server-1 ~]$ cd /home/techdocs/
[dev2@redhat9-server-1 techdocs]$ echo "This is the first tech doc." > techdoc1.txt 
[dev2@redhat9-server-1 techdocs]$ cat techdoc1.txt 
This is the first tech doc.

```
4. Xác minh rằng chỉ chủ sở hữu của tệp `techdoc1.txt` mới có thể xóa tệp đó. Nếu không phải là người dùng sở hữu tệp, hãy thử xóa tệp. Sau đó, chuyển sang chủ sở hữu, sao lưu tệp và xóa tệp. Cuối cùng, khôi phục tệp gốc từ bản sao lưu bằng cách di chuyển tệp.  

4.1 Với tư cách là người dùng dev2, hãy thử xóa tệp techdoc1.txt
```
[dev2@redhat9-server-1 techdocs]$ rm techdoc1.txt 
rm: cannot remove 'techdoc1.txt': Operation not permitted
[dev2@redhat9-server-1 techdocs]$ ll
total 4
-rw-rw-r--. 1 dev1 techdocs 28 Aug 13 15:20 techdoc1.txt

```
4.2 Thoát khỏi shell người dùng dev2. Chuyển sang người dùng dev1. Chuyển đến thư mục /home/techdocs.

```
[dev2@redhat9-server-1 techdocs]$ exit
logout
[root@redhat9-server-1 ~]# su - dev1
[dev1@redhat9-server-1 ~]$ cd /home/techdocs/

```
4.3 Sao lưu tệp `techdoc1.txt`, sau đó xóa tệp này.

```
[dev1@redhat9-server-1 techdocs]$ cp -pr techdoc1.txt techdoc1.txt.bkp
[dev1@redhat9-server-1 techdocs]$ ls
techdoc1.txt  techdoc1.txt.bkp
[dev1@redhat9-server-1 techdocs]$ rm techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ ll
total 4
-rw-rw-r--. 1 dev1 techdocs 28 Aug 13 15:20 techdoc1.txt.bkp

```
Ý nghĩa từng option
- -p (preserve – giữ nguyên thông tin gốc của file):
  Khi copy, nó sẽ giữ lại:
  - Thời gian chỉnh sửa (mtime) và thời gian truy cập (atime)
  - Quyền (permission)
  - Chủ sở hữu (owner) và nhóm (group) nếu bạn có quyền
- Không có -p: file copy sẽ có thời gian tạo mới và có thể quyền khác.
- -r (recursive – đệ quy):
  - Copy thư mục và toàn bộ nội dung bên trong (bao gồm cả thư mục con, file con).
  - Nếu copy chỉ một file, -r gần như không có tác dụng.

4.4 Khôi phục tệp techdoc1.txt từ bản sao lưu.

```
[dev1@redhat9-server-1 techdocs]$  mv techdoc1.txt.bkp techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ ll
total 4
-rw-rw-r--. 1 dev1 techdocs 28 Aug 13 15:20 techdoc1.txt

```

5. Trong thư mục `/home/techdocs`, hãy tạo các thư mục con cho người dùng dev1 và dev2 dựa trên bảng sau.

Chỉnh sửa quyền để chỉ chủ sở hữu mới có thể ghi vào các tệp và thư mục, nhóm `techdocs` có thể đọc chúng và những người khác không có quyền truy cập. Điều chỉnh quyền sở hữu và quyền cho phù hợp, đồng thời xác minh rằng các tệp mới tuân thủ các quy tắc truy cập này.

Owner	|Directory|	Group	|Files
---|---|---|---
dev1	|dev1	|techdocs	|dev1.txt <br> dev1.log <br> dev1.cfg
dev2	|dev2	|techdocs	|dev2.txt <br> dev2.log <br>dev2.cfg

5.1 Với tư cách là người dùng dev1, hãy tạo thư mục dev1. Đổi quyền sở hữu nhóm cho thư mục dev1 thành nhóm `techdocs`. Thiết lập quyền đọc, ghi và thực thi cho người dùng, quyền đọc và thực thi cho nhóm, và không cấp quyền cho những người khác trên thư mục dev1.
```
[dev1@redhat9-server-1 techdocs]$ ls
techdoc1.txt
[dev1@redhat9-server-1 techdocs]$ mkdir dev1
[dev1@redhat9-server-1 techdocs]$ chown :techdocs dev1
[dev1@redhat9-server-1 techdocs]$ chmod 0750 dev1
[dev1@redhat9-server-1 techdocs]$ ll
total 4
drwxr-x---. 2 dev1 techdocs  6 Aug 13 15:42 dev1
-rw-rw-r--. 1 dev1 techdocs 28 Aug 13 15:20 techdoc1.txt

```

5.2 Chuyển đến thư mục dev1. Liệt kê giá trị umask cho người dùng dev1. Đổi umask mặc định cho người dùng dev1 thành 0027.

```
[dev1@redhat9-server-1 techdocs]$ cd dev1/
[dev1@redhat9-server-1 dev1]$ umask
0022
[dev1@redhat9-server-1 dev1]$ umask 0027

```

5.3 Tạo các tệp được liệt kê trong bảng trước cho người dùng dev1. Đổi quyền sở hữu nhóm của các tệp này thành nhóm `techdocs`. Xác minh rằng người dùng dev1 có thể ghi vào chúng.
```
[dev1@redhat9-server-1 dev1]$ touch dev1.txt
[dev1@redhat9-server-1 dev1]$ touch dev1.log
[dev1@redhat9-server-1 dev1]$ touch dev1.cfg
[dev1@redhat9-server-1 dev1]$ chown :techdocs dev1.*
[dev1@redhat9-server-1 dev1]$ ll
total 0
-rw-r-----. 1 dev1 techdocs 0 Aug 13 15:50 dev1.cfg
-rw-r-----. 1 dev1 techdocs 0 Aug 13 15:50 dev1.log
-rw-r-----. 1 dev1 techdocs 0 Aug 13 15:50 dev1.txt
[dev1@redhat9-server-1 dev1]$ echo "hello" > dev1.txt 
[dev1@redhat9-server-1 dev1]$ cat dev1.txt 
hello

```

5.4 Thoát khỏi shell người dùng dev1. Chuyển sang người dùng dev2. Chuyển đến thư mục /home/techdocs.

```
[dev1@redhat9-server-1 dev1]$ exit
logout
[root@redhat9-server-1 ~]# su - dev2
[dev2@redhat9-server-1 ~]$ cd /home/techdocs/
```

5.5 Với tư cách là người dùng dev2, hãy tạo thư mục dev2. Đổi quyền sở hữu nhóm cho thư mục dev2 thành nhóm `techdocs`. Thiết lập quyền đọc, quyền ghi và quyền thực thi cho người dùng, quyền đọc và quyền thực thi cho nhóm, và không cấp quyền cho những người khác trên thư mục dev2.
```
[dev2@redhat9-server-1 techdocs]$ mkdir dev2
[dev2@redhat9-server-1 techdocs]$ chown :techdocs dev2
[dev2@redhat9-server-1 techdocs]$ chmod 0750 dev2
[dev2@redhat9-server-1 techdocs]$ ll
total 4
drwxr-x---. 2 dev1 techdocs 54 Aug 13 15:50 dev1
drwxr-x---. 2 dev2 techdocs  6 Aug 13 15:54 dev2
-rw-rw-r--. 1 dev1 techdocs 28 Aug 13 15:20 techdoc1.txt

```

5.6 Chuyển đến thư mục dev2. Liệt kê giá trị umask cho người dùng dev2. Đổi umask mặc định cho người dùng dev2 thành 0027.
```
[dev2@redhat9-server-1 techdocs]$ cd dev2/
[dev2@redhat9-server-1 dev2]$ umask
0022
[dev2@redhat9-server-1 dev2]$ umask 0027

```

5.7 Tạo các tệp được liệt kê trong bảng trước cho người dùng dev2. Đổi quyền sở hữu nhóm cho các tệp này thành nhóm techdocs. Xác minh rằng người dùng dev2 có thể ghi vào chúng.

```
[dev2@redhat9-server-1 dev2]$ touch dev2.txt \
> dev2.log dev2.cfg
[dev2@redhat9-server-1 dev2]$ ll
total 0
-rw-r-----. 1 dev2 techdocs 0 Aug 13 15:58 dev2.cfg
-rw-r-----. 1 dev2 techdocs 0 Aug 13 15:56 dev2.log
-rw-r-----. 1 dev2 techdocs 0 Aug 13 15:56 dev2.txt
[dev2@redhat9-server-1 dev2]$ chown :techdocs dev2.*
[dev2@redhat9-server-1 dev2]$ ll
total 0
-rw-r-----. 1 dev2 techdocs 0 Aug 13 15:58 dev2.cfg
-rw-r-----. 1 dev2 techdocs 0 Aug 13 15:56 dev2.log
-rw-r-----. 1 dev2 techdocs 0 Aug 13 15:56 dev2.txt
[dev2@redhat9-server-1 dev2]$ echo "hello" > dev2.txt
[dev2@redhat9-server-1 dev2]$ cat dev2.txt 
hello

```

6. Xác minh rằng người dùng trong nhóm techdocs không phải là chủ sở hữu tệp có thể đọc nội dung tệp trong thư mục cộng tác /home/techdocs. Với tư cách là người dùng editor1, hãy đọc các tệp thuộc sở hữu của những người dùng khác trong nhóm techdocs.
6.1 Thoát khỏi shell người dùng dev2. Chuyển sang người dùng editor1. Chuyển đến thư mục /home/techdocs.

```
[dev2@redhat9-server-1 dev2]$ exit
logout
[root@redhat9-server-1 ~]# su - editor1 
[editor1@redhat9-server-1 ~]$ cd /home/techdocs/

```
6.2 Xác định các thư mục và tệp mà người dùng editor1 không sở hữu. Xem cấu trúc tệp và chủ sở hữu trong thư mục cộng tác /home/techdocs.
```
[editor1@redhat9-server-1 techdocs]$ tree -ug
.
├── [dev1     techdocs]  dev1
│   ├── [dev1     techdocs]  dev1.cfg
│   ├── [dev1     techdocs]  dev1.log
│   └── [dev1     techdocs]  dev1.txt
├── [dev2     techdocs]  dev2
│   ├── [dev2     techdocs]  dev2.cfg
│   ├── [dev2     techdocs]  dev2.log
│   └── [dev2     techdocs]  dev2.txt
└── [dev1     techdocs]  techdoc1.txt

2 directories, 7 files
```
6.3 Với tư cách là người dùng editor1, hãy đọc nội dung trong các tệp techdoc1.txt, dev1.txt và dev2.txt.
```
[editor1@redhat9-server-1 techdocs]$ ll
total 4
drwxr-x---. 2 dev1 techdocs 54 Aug 13 15:50 dev1
drwxr-x---. 2 dev2 techdocs 54 Aug 13 15:58 dev2
-rw-rw-r--. 1 dev1 techdocs 28 Aug 13 15:20 techdoc1.txt
[editor1@redhat9-server-1 techdocs]$ cat techdoc1.txt 
This is the first tech doc.
[editor1@redhat9-server-1 techdocs]$ cat dev1/dev1.txt 
hello
[editor1@redhat9-server-1 techdocs]$ cat dev2/dev2.txt 
hello

```
7. Xác minh rằng chỉ những người dùng trong nhóm techdocs mới có thể truy cập thư mục cộng tác /home/techdocs. Với tư cách là người dùng dbadmin1, hãy thử truy cập thư mục /home/techdocs.

7.1 Thoát khỏi shell người dùng editor1. Chuyển sang người dùng dbadmin1. Chuyển đến thư mục /home/techdocs.

```

[editor1@redhat9-server-1 techdocs]$ exit
logout
[root@redhat9-server-1 ~]# su - dbadmin1 
[dbadmin1@redhat9-server-1 ~]$ cd /home/techdocs/
-bash: cd: /home/techdocs/: Permission denied
[dbadmin1@redhat9-server-1 ~]$ 

```
7.2 Thoát khỏi shell người dùng dbadmin1.

```
[dbadmin1@redhat9-server-1 ~]$ exit
logout
[root@redhat9-server-1 ~]# 
 
```

# CHAPTER 12: Install and Update Software with RPM
12.7 PAGE 71/128  
Tải xuống, cài đặt, cập nhật và quản lý các gói phần mềm từ kho lưu trữ gói Red Hat và DNF.

Kết quả
- Quản lý kho phần mềm.
- Cài đặt và nâng cấp các gói từ kho lưu trữ.
- Cài đặt gói RPM.

1. Trên máy chủ, hãy cấu hình kho phần mềm tùy chỉnh để cài đặt các gói cụ thể. Đặt tên kho lưu trữ là `errata` và tạo tệp kho lưu trữ `errata.repo`. Cấu hình tệp `errata.repo` để sử dụng kho lưu trữ http://repo.example.com/rhel10.0/x86_64/rhcsa-practice/errata. Không xác minh chữ ký GPG.

- Tuy thuoc vao version redhat
- Chi lab duoc tren tren moi truong redhat vi la repo private

```
[root@redhat9-server-1 ~]# vi /etc/yum.repos.d/errata.repo
---
[errata]
name=Custom Errata Repository
baseurl=http://repo.example.com/rhel10.0/x86_64/rhcsa-practice/errata
enabled=1
gpgcheck=0
---

---
[errata]
name=Red Hat Updates
baseurl=http://content.example.com/rhel9.3/x86_64/rhcsa-practice/errata
enabled=1
gpgcheck=0
---
```
2. Trên máy chủ, hãy cài đặt gói `rht-system`  

2.1 Liệt kê các gói có sẵn cho gói rht-system.
```
dnf list rht-system
```
2.2 Cài đặt phiên bản mới nhất của gói rht-system.
```
dnf install rht-system
```

3. Vì lý do bảo mật, máy chủ ServerB không được phép kết nối với máy in giấy. Bạn có thể thực hiện việc này bằng cách gỡ bỏ gói cups. Khi hoàn tất, hãy thoát khỏi root shell.

3.1 Liệt kê các gói cups đã cài đặt.

```
[root@redhat9-server-1 ~]# dnf list cups
Last metadata expiration check: 0:08:02 ago on Wed 27 Apr 2022 05:01:59 AM EDT.
Installed Packages
cups.x86_64        1:2.3.3op2-13.el9      @rhel-9.0-for-x86_64-appstream-rpms
```
3.2 Remove the cups package.
```
dnf remove cups.x86_64
```

4. Tập lệnh khởi động sẽ tải xuống gói `rhcsa-script-1.0.0-1.noarch.rpm` trong thư mục /`home/student` trên máy serverb.

Hãy xác nhận rằng gói `rhcsa-script-1.0.0-1.noarch.rpm` có sẵn trên serverb và cài đặt nó bằng quyền root. Kiểm tra xem gói đã được cài đặt chưa. Thoát khỏi máy serverb.

4.1 Xác minh rằng gói `rhcsa-script-1.0.0-1.noarch.rpm` có sẵn trên serverb.

```
[student@serverb ~]$ rpm -q -p rhcsa-script-1.0.0-1.noarch.rpm -i
Name        : rhcsa-script
Version     : 1.0.0
Release     : 1
Architecture: noarch
Install Date: (not installed)
Group       : System
Size        : 593
License     : GPL
Signature   : (none)
Source RPM  : rhcsa-script-1.0.0-1.src.rpm
Build Date  : Wed 23 Mar 2022 08:24:21 AM EDT
Build Host  : localhost
Packager    : Bernardo Gargallo
URL         : http://example.com
Summary     : RHCSA Practice Script
Description :
A RHCSA practice script.
The package changes the motd.
```

4.2 Install the rhcsa-script-1.0.0-1.noarch.rpm package.
```
[student@serverb ~]$ sudo dnf install \
rhcsa-script-1.0.0-1.noarch.rpm
[sudo] password for student: student
```

4.3 Verify that the package is installed.
```
[student@serverb ~]$ rpm -q rhcsa-script
rhcsa-script-1.0.0-1.noarch
[student@serverb ~]$
```
4.4 Trở lại hệ thống máy trạm với tư cách là người dùng là sinh viên.
```
[student@serverb ~]$ exit
logout
Connection to serverb closed.
[student@workstation ~]$
```

---
# CHAPTER 14: Access Removable Media
14.7 PAGE 85/128  
Truy cập hệ thống tệp trên các thiết bị lưu trữ di động bằng cách gắn chúng vào một thư mục trong hệ thống phân cấp tệp.

Kết quả
- Gắn hệ thống tệp.
- Tạo báo cáo sử dụng đĩa.
- Tìm tệp trong hệ thống tệp cục bộ.

1. Với tư cách là người dùng root trên máy serverb, hãy xác định UUID cho device `/dev/sdb1` và gắn kết nó bằng cách sử dụng UUID của nó trên thư mục `/mnt/system-report`.

1.1 Đăng nhập vào máy chủ serverb với tư cách là người dùng student và chuyển sang người dùng root. Sử dụng student làm mật khẩu.
```
student@workstation:~$ ssh student@serverb
...output omitted...
[student@serverb ~]$ sudo -i
[sudo] password for student: student
[root@serverb ~]#
```

1.2 Truy vấn UUID của thiết bị /dev/sdb1.
```
[root@redhat9-server-1 ~]# lsblk -fp /dev/sdb
NAME        FSTYPE FSVER LABEL UUID          FSAVAIL FSUSE% MOUNTPOINTS
/dev/sdb
└─/dev/sdb1 xfs                48bd5...3337a
```
Note: UUID có thể thay đổi tùy theo môi trường của bạn.

1.3 Kiểm tra xem thư mục `/mnt/system-report` có tồn tại không.

```
[root@redhat9-server-1 ~]# ls /mnt/system-report
ls: cannot access '/mnt/system-report': No such file or directory
```
1.4  Create the /mnt/system-report directory.
```
mkdir /mnt/system-report
```
1.5  Gắn thiết bị `/dev/sdb1` vào thư mục `/mnt/system-report` bằng cách sử dụng UUID. Thay thế UUID giữ chỗ trong lệnh sau bằng UUID từ môi trường của bạn.

```
mount UUID="48bd5...3337a" /mnt/system-report
```

Tren may ca nhan uuid
```
[root@redhat9-server-1 ~]# df -h
Filesystem             Size  Used Avail Use% Mounted on
devtmpfs               4.0M     0  4.0M   0% /dev
tmpfs                  870M     0  870M   0% /dev/shm
tmpfs                  348M  7.3M  341M   3% /run
efivarfs               256K   56K  196K  23% /sys/firmware/efi/efivars
/dev/mapper/rhel-root   22G  5.9G   16G  28% /
/dev/nvme0n1p2         960M  357M  604M  38% /boot
/dev/nvme0n1p1         599M  7.1M  592M   2% /boot/efi
tmpfs                  174M   52K  174M   1% /run/user/42
tmpfs                  174M   36K  174M   1% /run/user/0
[root@redhat9-server-1 ~]# lsblk -fp /dev/mapper/rhel-root 
NAME                  FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
/dev/mapper/rhel-root xfs                131d0080-1a1b-4e25-be6b-dded50e4a185   15.5G    27% /
```

1.6 Xác minh rằng thiết bị /dev/sdb1 được gắn vào thư mục /mnt/system-report.

```
lsblk -fp /dev/sdb1
NAME      FSTYPE FSVER LABEL UUID           FSAVAIL FSUSE% MOUNTPOINTS
/dev/sdb1 xfs                48bd5...3337a     4.8G     3% /mnt/system-report
```

2. Tạo báo cáo sử dụng đĩa cho thư mục `/usr/share`. Lưu kết quả vào tệp `/mnt/system-report/disk-usage.txt`.

```
du /usr/share > /mnt/system-report/disk-usage.txt
```

3. Sử dụng lệnh `locate` để tìm tất cả các tệp khớp với từ khóa `rsyslog.conf` và lưu trữ kết quả trong tệp `/mnt/system-report/search1.txt`. Hiển thị giải pháp

3.1 Update the locate database.
```
updatedb
```
3.2 Sử dụng lệnh `locate` để tìm tất cả các tệp khớp với từ khóa `rsyslog.conf`. Lưu kết quả vào tệp `/mnt/system-report/search1.txt`.
```
locate rsyslog.conf > /mnt/system-report/search1.txt
```
4. Tìm kiếm tất cả các tệp trong thư mục `/usr/share` có kích thước lớn hơn 5 MB nhưng nhỏ hơn 10 MB. Lưu kết quả vào tệp `/mnt/system-report/search2.txt`

```
[root@redhat9-server-1 ~]# find /usr/share -size +5M -size -10M > \
/mnt/system-report/search2.txt
```

5. Sau khi hoàn thành nhiệm vụ của hoạt động này, hãy quay lại máy trạm với tư cách là người dùng là học viên.
```
[root@serverb ~]# exit
logout
[student@serverb ~]$ exit
logout
Connection to serverb closed.
student@workstation:~$
```

# CHAPTER 15: Monitor and Manage Linux Processes
15.9 PAGE 95/128

Diễn giải và giám sát các số liệu hệ thống, đồng thời nghiên cứu ý nghĩa của các phép đo đó để cải thiện hiệu suất hệ thống của bạn.

Kết quả
- Quản lý quy trình với Top như một công cụ quản lý quy trình.

1. Trên may workstation , hãy mở hai cửa sổ terminal cạnh nhau. Trong phần này, các terminal này được gọi là trái và phải. Trên mỗi cửa sổ terminal, hãy đăng nhập vào máy `serverb` với tư cách là user `student`.

Tạo tập lệnh `task101.sh` trong thư mục `/home/student/bin`. Tập lệnh `task101.sh` tạo ra tải CPU nhân tạo bằng cách thực hiện các phép tính số học liên tục.

1.2 Trong shell bên trái, tạo thư mục /home/student/bin.
```
 mkdir -p /home/student/bin
```
1.3 Trong shell terminal bên trái, hãy tạo tệp `task101.sh` trong thư mục `~/bin`. Tệp phải chứa nội dung sau.
```
#!/bin/bash
touch ~/bin/.$(basename $0)
while true; do
  var=1
  while [[ var -lt 50000 ]]; do
    var=$(($var+1))
  done
  sleep 1
done
```
1.4 Làm cho tập lệnh task101.sh có thể thực thi được.
```
chmod +x /home/student/bin/task101.sh
```

2. Trong terminal bên phải, theo dõi tất cả các tiến trình đang chạy trong máy serverb.  

2.1 Trong terminal bên phải, hãy chạy tiện ích trên cùng để theo dõi tất cả các tiến trình. Điều chỉnh cửa sổ sao cho cao nhất có thể để xem thêm thông tin. Tiếp tục chạy tiện ích trên cùng để kiểm tra mức sử dụng CPU và tải trung bình trong các bước sau.
```
top
```

3. Trong shell terminal bên trái, hãy kiểm tra số lượng CPU logic trên máy ảo. Chạy tập lệnh task101.sh ở chế độ nền.

3.1 Kiểm tra số lượng CPU logic.

```
[student@serverb ~]$ grep "model name" /proc/cpuinfo | wc -l
2
```
3.2 Chạy tập lệnh task101.sh ở chế độ nền. Tập lệnh `task101.sh` nằm trong thư mục con `~/bin`. Do vị trí này, biến môi trường PATH sẽ định vị tập lệnh mà không cần xác định đường dẫn đầy đủ của nó.

```
[student@serverb ~]$ task101.sh &
[1] 2608
```