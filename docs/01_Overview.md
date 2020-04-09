# Tổng quan về OMD
## **1) Giới thiệu về OMD - Checkmk** <img src=https://i.imgur.com/HhEPPqV.png width=35% align=right>
- **OMD - Open Monitoring Distribution** là một project được phát triển từ năm `2010` bới **Mathias Kettner**. 
- **OMD** sử dụng nhân là **Nagios Core**, kết hợp với các phần mềm mã nguồn mở khác để đóng gói thành một sản phẩm phục vụ cho nhu cầu giám sát, cảnh báo và hiển thị .
- **OMD (checkmk)** cung cấp các hình thức cảnh báo (email, sms, âm thanh, slack …) và triển khai theo mô hình phân tán.
- Trang chủ OMD : https://omdistro.org/

    <p align=center><img src=https://i.imgur.com/maEveIt.png width=70%></p>

- **Checkmk** là một phần của **OMD** 
- Trang chủ **Checkmk** : https://checkmk.com/
- Phiên bản mới nhất : `1.6`
## **2) Lịch sử hình thành của OMD - Checkmk**
- Dự án **Check_MK** được phát triển từ năm `2008` như là một plugin của **Nagios Core**.
- Năm `2010` dự án **OMD** (**Open Monitoring Distribution**) được khởi động bởi **Mathias Kettner**, là sự kết hợp của **Nagios**, **Check_MK**, **NagVis**, **PNP4Nagios**, **DocuWiki**, ... tạo nên sự linh hoạt trong giám sát. Các distro của **OMD** hiện tại đang là **OMD-LABS** và **CHECK_MK RAW**. Trong đó :
    - **OMD-Labs** chứa tất cả các thành phần nguyên bản của **OMD** và một số addons thêm vào như **Grafana**, **InfluxDB**, **Naemon**, **Icinga 2**.
    - **CHECK_MK RAW** tập trung vào **Check_MK**, đây là một gói nhỏ hơn chứa các thành phần để chạy **Check_MK**.
- Năm `2015`, phiên bản đơn giản của **OMD** ra đời gọi là **Check_MK**, tồn tại có 2 phiên bản là Check_MK Raw Edition (CRE) và Check_MK Enterprise Edition (CEE).
- Ngày `16.04.2019`, dự án **Check_MK** chính thức được đổi tên thành "**Checkmk**"
- Checkmk hiện có 4 phiên bản :
    - **Checkmk Raw Edition** : miễn phí, 100% mã nguồn mở, và là 1 hệ thống giám sát IT toàn diện
    - **Checkmk Enterprise - Free Edition** : phiên bản miễn phí của bản thương mại, giới hạn với 2 sites, mỗi site tối đa 10 host có thể giám sát .
    - **Checkmk Enterprise - Standard Edition** : bao gồm một số tính năng bổ sung có liên quan trong môi trường chuyên nghiệp, cung cấp khả năng mở rộng tốt hơn và cung cấp cơ hội nhận hỗ trợ cấp doanh nghiệp thông qua nhà cung cấp hoặc mạng lưới đối tác của mình. Hiện tại phiên bản này trả phí theo năm .
    - **Checkmk Enterprise - Managed Services Edition** dựa trên Enterprise - Standard Edition và cung cấp thêm các tính năng đặc biệt nhắm vào các công ty muốn cung cấp Checkmk dưới dạng dịch vụ được quản lý .


<p align=center><img src=https://i.imgur.com/QnP6nVa.png width=80%></p>

## **3) Các tính năng nổi bật của Checkmk**
- Giám sát toàn diện :
    - Máy chủ, thiết bị mạng, tường lửa, SAN…
    - Các nền tảng ảo hóa : VMWare, HyperV, OpenStack, XEN.
    - Ứng dụng : Web, Database, Mail…
- Giao diện trực quan :
    - Overview host và service, event và cảnh báo trên 1 dashboard.
    - Cung cấp network topology tự động.
- Giám sát phân tán Multi-site :
    - Tạo site giám sát theo vùng địa lý
    - Quản lý tập trung các site trên Web
- Mutitenancy :
    - Tạo nhiều tenant độc lập trên một hạ tầng
    - Quản lý tenant theo multi-user
- GUI cấu hình giám sát client
    - Giao diện web WATO : khai báo và cấu hình cho host giám sát
    - Quản trị và điều khiển việc giám sát trên Web.
- Cảnh báo linh hoạt :
    - Gửi cảnh báo bằng nhiều hình thức : SMS, Email, Slack
    - Cảnh báo phân cấp theo mức độ người dùng.
## **4) So sánh Checkmk với các giải pháp giám sát khác**
| Tiêu chí | Checkmk | Zabbix | Nagios |
|----------|---------|--------|--------|
| Máy chủ vật lý | &#10004; | &#10004; | &#10004; |
| Thiết bị mạng | &#10004; | &#10004; | &#10004; |
| Nền tảng ảo hóa | &#10004; | Hạn chế | Hạn chế |
| Hiển thị | &#10004; | &#10004; | Hạn chế |
| Cảnh báo | &#10004; | &#10004; | &#10004; |
| Multitenancy | &#10004; | Không có | Không có |
| Độ khó triển khai | Dễ | Trung bình | Khó |
| Độ khó sử dụng | Dễ | Trung bình | Khó |

- Sự đóng góp từ cộng đồng :

    <img src=https://i.imgur.com/sTrwCOM.png>

## **5) Kiến trúc tổng quan của OMD**
- **OMD** được xây dựng từ những đóng góp của cộng đồng về những khó khăn hay khuyết điểm mà **Nagios** gặp phải, từ đó đưa ra quyết định cần tích hợp thêm những sản phẩm gì để cải thiện .
- **Checkmk** ra đời để giải quyết bài toán về hiệu năng mà **Nagios** gặp phải trong quá khứ . Cơ chế mới của **Checkmk** cho phép việc mở rộng hệ thống trở nên dễ dàng hơn, có thể giám sát nhiều hệ thống chỉ từ một máy chủ **Nagios server** .
- Có 2 mô đun mà **Checkmk** sử dụng để cải thiện đáng kể hiệu năng là **Livestatus** và **Livecheck** .
    - Livestatus có những thay đổi để cải thiện hiệu năng đó là:
        - Livestatus cũng sử dụng Nagios Event Broker API như NDO, nhưng nó sẽ không chủ động ghi dữ liệu ra. Thay vào đó, nó sẽ mở ra một socket để dữ liệu có thể được lấy ra theo yêu cầu.
        - Livestatus tiêu tốn ít CPU.
        - Livestatus không làm cho Disk I/O thay đổi khi truy vấn trạng thái dữ liệu.
        - Không cần cấu hình. Không cần cơ sở dữ liệu. Không cần quản lý.
    - Livecheck hoạt động như thế nào để cải thiện được hiệu năng :
        - Livecheck sử dụng các helper process, các core giao tiếp với helper thông qua Unix socket (điều này không xảy ra trên file system).
        - Chỉ có một một helper program được fork thay vì toàn bộ Nagios Core.
        - Các tiến trình fork được phân tán trên tất cả các CPU thay vì chỉ một như trước.
        - Process VM size tổng chỉ khoảng 100KB.

<p align=center><img src=https://i.imgur.com/cByjiFa.png width=70%></p>

## **6) Các phương thức mà Checkmk sử dụng**

<p align=center><img src=https://i.imgur.com/b72XJtZ.png width=70%></p>

tmp/run/live: Unix socket mà qua đó các query và lệnh được submit
bin/lq: Lệnh script để đơn giản hóa việc ban hành các query và lệnh cho Unix-Socket trong Livestatus.
var/log/nagios.log: File log của Nagios core (Phiên bản Check_MK Raw Edition)
var/nagios/debug.log: file log của Nagios debug
var/nagios/archive/: Các file log history sẽ được lưu ở đây. Chỉ được đọc nếu cần thiết
var/log/cmc.log: file log của Checkmk Microcore (Phiên bản Check_MK Enterprise Edition)
var/check_mk/core/history: file log của Checkmk Microcore. Tất cả thay đổi trong quá trình core hoạt động sẽ ở đây. Ví dụ: một host/service thay đổi state
var/check_mk/core/archive/: Các file log history sẽ được lưu ở đây. Chỉ được đọc nếu cần thiết
var/log/notify.log: file log của Notification module (module thông báo).
var/log/mknotifyd.log: File log của Notification spooler
var/check_mk/notify.log: File log của Notification scripts.
share/doc/check_mk/livestatus/LQL-examples/: Trong thư mục này, có thể tìm thấy một số ví dụ về các truy vấn Livestatus mà bạn có thể thử
share/doc/check_mk/livestatus/api/python: API cho Python và các ví dụ.
share/doc/check_mk/livestatus/api/per: API cho Perl và các ví dụ.
share/doc/check_mk/livestatus/api/c++: Các ví dụ cho C++
/var/log/mail.log: log của SMTP server trên Debian và Ubuntu



