# Tổng quan về Linux  
### Tổng quan  
**Kiến trúc**  
Kiến trúc hệ điều hành Linux chia làm 3 phần:  
- Kernel  
- Shell  
- Applications  
**Thành phần**  
- **Kernel** : Đây là phần quan trọng của HĐH, phần kernel chứa các module, thư viện để quản lý và giao tiếp với phần cứng và các ứng dụng.  
Dùng để quản lý phần cứng và các ứng dụng thực thi  
+ Linux xem mỗi thiết bị phần cứng tương đương với một tập tin  
+ Khi khởi động máy tính, Kernel được nạp vào trong bộ nhớ chính, và nó hoạt động cho đến khi tắt máy. Thực hiện chức năng mức thấp và chức năng mức hệ thống.  
+ Kernel chịu trách nhiệm thông dịch và gửi các chỉ thị tới bộ vi xử lý máy tính.  
+ Kernel cũng chịu trách nhiệm về các tiến trình và cung cấp các đầu vào và ra cho các tiến trình.  
- **Shell** : là một chương trình có chức năng thực thi các lệnh từ người dùng hoặc từ các ứng dụng – tiện ích yêu cầu chuyển đến cho Kernel xử lý.  
Các loại shell:  
+ Sh (the Bourne Shell)  
+ Bash(Bourne-again shell): đây là shell mặc định trên linux.
+ csh (the C shell): shell được viết bằng ngôn ngữ lập trình C.  
+ ash (the Almquist shell).  
+ tsh (the TENEX C shell).  
+ zsh (the Z shell).  
Chức năng của Shell:  
+ Thông dịch lệnh.  
+ Khởi tạo chương trình.  
+ Định hướng vào ra.  
+ Kết nối đường ống.  
+ Thao tác trên tập tin.  
+ Duy trì các biến.  
+ Điều khiển môi trường.  
+ Lập trình shell.  
- **Applications**: Là các ứng dụng và tiện ích mà người dùng cài đặt trên Server. Ví dụ: ftp, samba, Proxy, …  
- **Commands and Utilities**: 
Khi làm việc với HĐH Linux hầu như tất cả người dùng đều sử dụng lệnh để làm việc. Lệnh là một chương trình hoặc một script dùng để thực thi một nhiệm vụ nào đó. Lệnh được gõ sau dấu nhắc shell.  
Dòng lệnh shell tổng quát có dạng như sau: command [options]  
Trong đó:  
+ command Lệnh  
+ options Tùy chọn, thường bắt đầu bằng - hoặc - -.Nhiều tùy chọn có thể kết hợp bằng một ký hiệu -. Ví dụ: -lF thay vì -l -F  
Dòng lệnh shell có phân biệt chữ thường và chữ hoa.  
***Các lệnh cơ bản :***  
1. Lệnh trợ giúp: man  
Mỗi lệnh trên Linux có rất nhiều options, mỗi options thực hiện các chức năng khác 
nhau, người quản trị cũng không cần nhớ hết các options của lệnh mà chỉ cần nhớ một vài options thông dụng. Để biết một lệnh có bao nhiêu options cũng như chức năng của từng options thì lệnh đầu tiên cần phải biết là lệnh: man  
Cấu trúc lệnh: $ man <tên lệnh>  
Ví dụ: Để xem hướng dẫn sử dụng lệnh cp (copy) có thể nhập lệnh $man cp  
Để thoát khỏi man ta bấm phím “q”  
2. Các lệnh kiểm tra performance  
Lệnh xem thông tin RAM: Xem tổng dung lượng, dung lượng hiện tại đang dùng, dung lượng còn trống. có 2 lệnh đó là:  
- cat /proc/meminfo  
Lệnh cat: Dùng để đọc nội dung của file text  
/proc/meminfo: đây là đường dẫn (đường dẫn tuyệt đối) tới file chứa thông tin  
RAM có tên là meminfo  
- free  
các options:  
- -b Hiển thị theo bytes  
- -k Hiển thị theo kilobytes  
- -m Hiển thị theo megabytes  
- -g Hiển thị theo gigabytes  
- --tera Hiển thị theo terabytes  
- -h Hiển thị theo kiểu tự động gom theo khối dữ liệu  
Lệnh xem thông tin CPU: cat /proc/cpuinfo  
Lệnh hiển thị thông tin kernel: uname -a  
options:  
-a : all information  
Lệnh hiển thị phiên bản Centos: cat /etc/redhat-release  
Lệnh xem dung lượng ổ cứng: Xem dung lượng ổ cứng đã dùng và còn trống bao nhiêu: df-h  
options: 
 -h : in kích thước mà người dùng có thể đọc  
Lệnh xem các tiến trình: top  
Lệnh xem dung lượng của thư mục: du  
options:  
- -s : xuất kết quả theo summarize (tổng dung lượng)  
- -h: in kích thước mà người dùng có thể đọc  
Ví dụ: Xem dung lượng của thư mục /etc  
 du -sh /etc  
Lệnh xem tên server: hostname  
Lệnh xem địa chỉ ip: ifconfig  
**Quản lý thư mục, Quản lý tập tin**  
Lệnh ls: dùng để xem (liệt kê) nội dung thư mục  
Cấu trúc lệnh: ls [options] [Path]  
Options:  
-  -l: liệt kê chi tiết.  
-  -a: liệt kê tất cả các file ẩn   
Lệnh cd: dùng để chuyển thư mục  
Cấu trúc lệnh: cd[Path]  
Ví dụ: cd /etc Chuyển đến thư mục /etc.  
cd usr Chuyển vào thư mục usr là con của thư mục hiện hành.  
cd .. Chuyển lên thư mục cấp cao hơn (cha)  
cd Chuyển về thư mục home  
cd ~ Chuyển về thư mục home  
Lệnh pwd: cho biết thư mục hiện hành  
Cấu trúc lệnh: pwd  
Lệnh passwd: đổi mật khẩu đăng nhập của user đang login.  
Cấu trúc lệnh: passwd  
Lệnh cp: dùng để sao chép file  
Cấu trúc lệnh: cp [Options] Source Dest  
Options:  
-R, -r : Sao chép toàn bộ thư mục.  
Lệnh mv: dùng để đổi tên / di chuyển thư mục hoặc file từ nơi này sang nơi khác  
Cấu trúc lệnh: mv [options] Source Dest  
Options:  
-i : Nhắc trước khi di chuyển với tập tin/thư mục đích đã có rồi.  
-f: Ghi đè khi di chuyển với tập tin/thư mục đích đã có rồi.  
Lệnh mkdir: dùng để tạo thư mục  
Cấu trúc lệnh: mkdir [Options] Directory  
Options:  
-p : Cho phép tạo thư mục con ngay cả khi chưa có thư mục cha.  
Directory: Tên thư mục muốn tạo.  
Lệnh rmdir: dùng để xóa thư mục rỗng. Thư mục rỗng là thư mục không chứa bất kỳ
thành phần nào.  
Cấu trúc lệnh: rmdir [options] directory  
Options:  
-p : xóa thư mục và cả thư mục cha.  
Directory: Tên thư mục muốn xóa.  
Lệnh rm: dùng để xoá file/thư mục. Lệnh này được xem là một trong những lệnh nguy hiểm của Linux.  
Cấu trúc lệnh: rm [options] file  
Options:  
-f: xóa không cần hỏi  
-i: hỏi trước khi xóa  
-R, -r: xóa toàn bộ thư mục, kể cả thư mục con  
Lệnh gedit: dùng để mở file với trình soạn thảo gedit.  
Lệnh vi: dùng để mở trình soạn thảo thảo vi.  
**Vim & nano editor:**  
Là trình soan thảo Terminal  
***GNU nano***
Các tính năng của GNU nano bao gồm:  
+ Hỗ trợ Autoconf  
+ Chức năng tìm kiếm phân biệt chữ hoa chữ thường  
+ Tìm kiếm và thay thế tương tác  
+ Khả năng tự động thụt lề  
+ Tùy chọn chiều rộng tab được hiển thị  
+ Tìm kiếm và thay thế biểu thức thông thường  
+ Công tắc chuyển đổi cho flag cmdline thông qua các phím meta  
+ Tab completion (nhấn Tab trong khi nhập lệnh, tùy chọn hoặc tên file và môi trường shell sẽ tự động hoàn thành những gì bạn đang nhập) khi đọc/ghi file  
+ Soft text wrapping (văn bản trông giống như đã kết thúc ở phần rìa màn hình, nhưng thực tế, nó là một dòng rất dài. Các phần tiếp theo được chỉ định bằng $)  
***Vim***  
Vim có lợi thế là mạnh hơn GNU nano. Vim không chỉ chứa nhiều tính năng hơn, mà bạn 
còn có thể tùy chỉnh chương trình với các plugin và script.  
Các tính năng của Vim bao gồm:  
+ Các lệnh tự động  
+ Các lệnh bổ sung  
+ Đầu vào  
+ Giới hạn bộ nhớ cao hơn vanilla vi  
+ Chia nhỏ màn hình  
+ Khôi phục phiên  
+ Mở rộng tab  
+ Hệ thống tag  
+ Thêm màu cho cú pháp  




