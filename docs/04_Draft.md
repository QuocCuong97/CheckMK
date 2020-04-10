
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

