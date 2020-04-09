# Các thành phần của Checkmk

## **1) Cấu hình và check engine**
- Checkmk cung cấp phương pháp để cấu hình nền tảng giám sát của ta một cách độc lập với core giám sát ta đang sử dụng. ta không cần xử lý trực tiếp các tệp cấu hình thông thường vì checkmk đảm nhiệm việc tìm dịch vụ tự động và cấu hình thực tế. Checkmk cung cấp một phương pháp hiệu quả cao để thực hiện kiểm tra: host của ta chỉ được liên lạc một lần cho mỗi lần kiểm tra. Kết quả sau đó được gửi đến core giám sát dưới dạng passive check. Giảm đáng kể việc sử dụng tài nguyên. 
- So sánh với các nền tảng giám sát khác: Đối với mỗi lần kiểm tra (thường là một lần mỗi phút), ta thăm dò host của mình cho mọi số liệu mà ta quan tâm. Vì vậy, nếu ta có một CPU và một ổ cứng, sẽ có hai lần kiểm tra mỗi phút (một cho CPU và một cho ổ cứng). Checkmk lấy được cả hai số liệu trong một lần check. Bây giờ hãy tưởng tượng rằng ta có hàng ngàn máy chủ lưu trữ và lên đến hàng trăm nghìn kiểm tra, check_mk sẽ giúp ta tiết kiệm tài nguyên đáng kể.
- Checkmk đảm nhiệm việc tạo dữ liệu cấu hình hoàn chỉnh cần thiết cho hệ thống giám sát của ta. Ta có thể tập trung vào việc thu thập thông tin quan trọng đối với ta và bớt phải quan tâm vào các file cấu hình. 
## **2) Livecheck**
- Livecheck sử dụng các helper process, các core giao tiếp với helper thông qua Unix socket (điều này không xảy ra trên file system)
- Chỉ có một một helper program được fork thay vì toàn bộ Nagios Core.
- Mỗi khi kiểm tra dịch vụ sẽ không chỉ một CPU làm việc như trước đây mà được phân tán trên tất cả các core của CPU.
## **3) Livestatus**
- Livestatus là interface quan trọng nhất trong Checkmk. Đây là cách nhanh nhất có thể để lấy tất cả dữ liệu của máy chủ và dịch vụ được giám sát. Dữ liệu được đọc trực tiếp từ RAM, tránh truy cập vào ổ cứng, do đó cung cấp quyền truy cập nhanh vào giám sát mà không gây quá nhiều tải cho hệ thống. Cú pháp của livestatus dựa trên giao thức HTTP. 
- Để cấu trúc dữ liệu, dữ liệu được sắp xếp theo bảng và cột. Bằng cách này, dữ liệu có thể được tìm kiếm và lấy ra một cách đơn giản.
    - Livestatus sẽ mở ra một socket để dữ liệu có thể được lấy ra theo yêu cầu. 
    - Livestatus tiêu tốn ít CPU
    - Livestatus không làm cho Disk I/O thay đổi khi truy vấn trạng thái dữ liệu
    - Không cần cấu hình. Không cần cơ sở dữ liệu. Không cần quản lý
    - Livestatus có quy mô lớn với hơn 50.000 dịch vụ

https://checkmk.com/cms_livestatus.html

#### Giám sát phân tán với Livestatus

**Central status (tình trạng trung tâm)**: 

Livestatus là một giao diện được tích hợp vào core giám sát cho phép các chương trình bên ngoài khác truy vấn dữ liệu trạng thái và thực thi các lệnh. Livestatus có thể được cung cấp qua mạng để nó có thể được truy cập bởi Checkmk-instance từ xa. Giao diện người dùng Checkmk sử dụng livestatus để kết hợp tất cả các instance (site) vào một tổng quan chung. Điều này cho cảm giác giống như một hệ thống giám sát lớn.

Sơ đồ sau cho thấy cấu trúc của một giám sát với livestatus được phân phối trên ba địa điểm. Checkmk-instance *Master Site* là site xử lý trung tâm. Từ đây hệ thống trung tâm sẽ được kiểm soát trực tiếp. Ngoài ra, còn có Slave Site 1 và Slave Site 2 được đặt trong các mạng khác và được kiểm soát bởi các hệ thống cục bộ của chúng:

![Imgur](https://i.imgur.com/yuRovMc.png)

Điều làm cho phương pháp này trở nên đặc biệt là trạng thái giám sát của Slave không được gửi liên tục đến Master. GUI luôn chỉ lấy dữ liệu live từ các instance từ xa khi người dùng yêu cầu trong trung tâm điều khiển. Dưới đây là một số ưu điểm của phương pháp này:

- Khả năng mở rộng (Scalability): Bản thân việc giám sát không tạo ra lưu lượng mạng giữa master và slave. Theo cách này, hàng trăm location, hoặc nhiều hơn, có thể được kết nối.
- Độ tin cậy (Reliability): Nếu kết nối mạng với slave không thành công, giám sát cục bộ vẫn tiếp tục hoạt động bình thường. Không có lỗ hổng nào trong quá trình ghi dữ liệu và cũng không có dữ liệu bi kẹt (jam). Thông báo cục bộ sẽ vẫn hoạt động.
- Đơn giản (Simplicity): Các instance (site) có thể rất dễ dàng kết hợp hoặc loại bỏ.
- Tính linh hoạt (Flexibility): Các trường hợp slave vẫn khép kín (self-contained) vẫn có thể được sử dụng để vận hành ở location tương ứng của chúng.  

**Cấu hình trung tâm (Centralized configuration)**

Trong hệ thống phân phối sử dụng Livecheck như đã mô tả ở trên, các instance có thể được duy trì độc lập bởi các team khác nhau, và master chỉ có nhiệm vụ cung cấp một bảng điều khiển tập trung.

Trong trường hợp nhiều hoặc tất cả instance cần được cần được quản lý bởi 1 team, một cấu hình trung tâm sẽ dễ xử lý hơn nhiều. Checkmk hỗ trợ việc này với công cụ ‘distributed WATO’. Với điều này, tất cả các host, service, user, phân quyền, thông báo, v.v. sẽ được quản lý tập trung tại master bằng cách dùng WATO. Sau đó tùy vào task của chúng, sẽ được phân phối cho các slave.

Một hệ thống như vậy không chỉ có một tổng quan về trạng thái chung mà còn có một cấu hình chung, và mang lại cảm giác giống như một hệ thống lớn.

## **4) Multisite**
- Là một giao diện web cho người dùng tốt hơn để thay thế cho Nagios.
- Web-GUI Multisite có thể dùng được mà không cần cấu hình và check engine của checkmk. Nó là một giao diện web hiện đại với tốc độ tải trang nhanh cung cấp cấu hình người dùng có thể xác định (user-definable configurations), giám sát phân tán bằng cách tích hợp nhiều thực thể giám sát (Monitoring-entities) thông qua Livestatus, tích hợp của NagVis và PNP4Nagios, một kết nối LDAP đã được tích hợp, quyền truy cập vào Status Data qua các dịch vụ web và v.v... Multisite sử dụng Livestatus để truy cập vào Status Data.
- Nó có 2 chức năng:
    - Nó hiển thị trạng thái hiện tại của tất cả các host, service và event.
    - Sử dụng để cấu hình Checkmk. Module có liên quan được gọi là WATO.

https://checkmk.com/cms_user_interface.html

## **5) WATO (Web Administration Tool)**
- WATO làm cho việc quản trị toàn bộ hệ thống dựa trên Check_MK có thể thực hiện được qua trình duyệt. Điều này không chỉ không giới hạn trong việc quản lý host và service và các rule Check_MK, mà còn bao gồm quản lý user, role, groups, thời gian, Nagios-check và v.v. 
- Wato bao gồm nhiều modules. Mỗi modules có một chức năng khác nhau. Một số module phổ biến là: 
    - Main menu: chứa danh sách tất cả các loại modules cùng với một miêu tả ngắn cách dùng
    - Monitoring Agents: Tại đây sẽ tìm thấy các agent cho linux windowns và cách hệ điều hành khác
    - Host: Quản lý các host được theo dõi
    - Global Settings: Những cài đặt không áp dụng cho host, service và users
    - Host & Service Parameters : danh sách tất cả các cài đặt giám sát về host và service
- Khi thay đổi bằng WATO thì giám sát sẽ chưa thực hiện vai trò của mình ngay lập tức. Mà sau khi nhấn nút Activate changes mới thực hiện thay đổi và mới bắt đầu thực hiện giám sát.

https://checkmk.com/cms_wato.html#Important%20WATO%20modules

## **6) Cảnh báo**

Hệ thống cảnh báo mới của Checkmk làm cho cấu hình của thông báo đơn giản và linh hoạt. Nhiều hình thức (SMS, email, slack, ...) có thể được xác định và cấu hình khác nhau cho mỗi người dùng.

Cụ thể: https://checkmk.com/cms_notifications.html

## **7) Kinh doanh thông minh (Business Intelligence)**

Module này đã được tích hợp trong Multisite-GUI. Nó tổng hợp status data từ nhiều host và dịch vụ để cung cấp trạng thái hoàn chỉnh cho các ứng dụng phức tạp và các processs tương tự. Điều này cung cấp một cái nhìn tổng quan nhanh chóng cho người quản lý và người dùng. 

https://checkmk.com/cms_bi.html

## **8) Mobile**

Phiên bản mobile của Multisite-GUI được tối ưu hóa cho smartphone và cho phép truy cập đến tất cả status data. Các lệnh như Acknowledge và Set for Downtimes có thể được thực thi. Mobile-GUI tự động khả dụng khi Multisite được cài đặt. Các thiết bị di động được tự động nhận dạng.

## **9) Event Console**

Check_MK Event Console tích hợp việc xử lý thông điệp log và SNMP-Traps vào việc giám sát. Daemon của nó (mkeventd) được cấu hình thông qua một bộ Rule và xác định các thông điệp nào đến và được phần loại như thế nào. Event Console còn tận dụng một inbuilt Syslog-Daemon nhận các thông điệp trực tiếp từ port 514.

https://checkmk.com/cms_ec.html

## **10) Các tập tin và thư mục liên quan**

- `tmp/run/live`: Unix socket mà qua đó các query và lệnh được submit
- `bin/lq`: Lệnh script để đơn giản hóa việc ban hành các query và lệnh cho Unix-Socket trong Livestatus.
- `var/log/nagios.log`: File log của Nagios core (Phiên bản Check_MK Raw Edition)
- `var/nagios/debug.log`: file log của Nagios debug
- `var/nagios/archive/`: Các file log history sẽ được lưu ở đây. Chỉ được đọc nếu cần thiết
- `var/log/cmc.log`: file log của Checkmk Microcore (Phiên bản Check_MK Enterprise Edition)
- `var/check_mk/core/history`: file log của Checkmk Microcore. Tất cả thay đổi trong quá trình core hoạt động sẽ ở đây. Ví dụ: một host/service thay đổi state
- `var/check_mk/core/archive/`: Các file log history sẽ được lưu ở đây. Chỉ được đọc nếu cần thiết
- `var/log/notify.log`: file log của Notification module (module thông báo). 
- `var/log/mknotifyd.log`: File log của Notification spooler
- `var/check_mk/notify.log`: File log của Notification scripts.
- `share/doc/check_mk/livestatus/LQL-examples/`: Trong thư mục này, có thể tìm thấy một số ví dụ về các truy vấn Livestatus mà bạn có thể thử
- `share/doc/check_mk/livestatus/api/python`: API cho Python và các ví dụ.
- `share/doc/check_mk/livestatus/api/per`: API cho Perl và các ví dụ.
- `share/doc/check_mk/livestatus/api/c++`: Các ví dụ cho C++
- `/var/log/mail.log`: log của SMTP server trên Debian và Ubuntu

Xem thêm về các file và thư mục liên quan

https://checkmk.com/cms_livestatus.html#files

https://checkmk.com/cms_notifications.html#files
