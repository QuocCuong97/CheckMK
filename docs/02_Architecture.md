# Các thành phần trong kiến trúc của Checkmk

- Có rất nhiều thành phần tạo nên hệ thống giám sát **Checkmk** :
    <p align=center><img src=https://i.imgur.com/YVSG2v7.png width=80%></p>

### **1) Cấu hình và Check Engine**
- **Checkmk** cung cấp một phương pháp thông minh để cấu hình nền tảng giám sát độc lập với lõi giám sát mà người quản trị sử dụng .
- Người quản trị sẽ không cần phải làm việc với các file cấu hình thông thường vì **Checkmk** sẽ đảm nhiêm hết việc khám phá các dịch vụ một cách tự động và cấu hình một cách thực tế . 
- **Checkmk** cung cấp một phương pháp hiệu quả cao để thực hiện kiểm tra : các host bị giám sát chỉ được liên hệ một lần cho mỗi khoảng thời gian kiểm tra (***check interval***) . Kết quả sau đó được gửi đến lõi giám sát dưới dạng **passive check** => Giảm thiểu đáng kể việc sử dụng tài nguyên .
- So sánh việc giám sát trên với chức năng điển hình của **Nagios** ( và hầu hết các nền tảng giám sát khác ) : với mỗi khoảng thời gian kiểm tra (***check interval***) - thường là 1 lần/1 phút . Người quản trị sẽ thăm dò máy chủ của mình cho mọi số liệu mà họ quan tâm . Vì vậy, nếu có 1 CPU và 1 ổ HDD, sẽ có 2 lần kiểm tra cho mỗi phút ( 1 cho CPU và 1 cho ổ HDD ) . **Checkmk** chỉ cần 1 lần cũng sẽ có được cả 2 số liệu trên . Giả sử có hàng ngàn máy chủ cần giám sát, **checkmk** sẽ giúp tiết kiệm tài nguyên đáng kể .
- **Checkmk** đảm nhiệm việc tạo dữ liệu cấu hình hoàn chỉnh cần thiết cho hệ thống giám sát . Người quản trị có thể tập trung vào việc thu thập thông tin quan trọng và ít cần quan tâm hơn đến các file cấu hình .

[Tham khảo thêm](https://checkmk.com/cms.html)
### **2) Live Status**
- **Live Status** là thứ quan trọng nhất trong **Checkmk** . Đây là cách nhanh nhất có thể để lấy tất cả các dữ liệu trực tiếp của máy chủ và dịch vụ được giám sát, bao gồm cả dữ liệu trực tiếp .
- **Live Status** có những thay đổi để cải thiện hiệu năng đó là:
    - **Live Status** cũng sử dụng Nagios Event Broker API như NDO, nhưng nó sẽ không chủ động ghi dữ liệu ra. Thay vào đó, nó sẽ mở ra một socket để dữ liệu có thể được lấy ra theo yêu cầu.
    - **Live Status** tiêu tốn ít CPU.
    - **Live Status** không làm cho Disk I/O thay đổi khi truy vấn trạng thái dữ liệu.
    - Không cần cấu hình. Không cần cơ sở dữ liệu. Không cần quản lý.
- **Live Status** cung cấp kết nối trực tiếp đến **Status Data** trên các Host và các dịch vụ thông qua **UNIX-Socket** . Nó sẽ kích hoạt 1 số các addon như **NagVis** để truy cập nhanh và hiệu quả vào **Status Data** . Truy cập được cung cấp qua 1 giao thức đơn giản và có thể truy cập được từ tất cả các ngôn ngữ lập trình mà không cần thư viện đặc biệt - ngay cả từ shell .
- Các dữ liệu được lấy về được sắp xếp theo bảng và cột . Bảng các host bao gồm tên, trạng thái và nhiều cột khác,... Mỗi dòng trong bảng các host đại diện cho một máy chủ, một dịch vụ,... Bằng cách này, dữ liệu có thể được tìm kiếm và lấy ra 1 cách đơn giản .
- **Status Data** có thể được lấy về thủ công bằng **LQL - LiveStatus Query Language** bằng 3 cách :
    - Sử dụng **LQL** trong Shell
    - Sử dụng **LQL** trong Python
    - Sử dụng **Livestatus-API** đối với các ngôn ngữ Python, Perl, C++

[Tham khảo thêm](https://checkmk.com/cms_livestatus.html)
### **3) Multisite**
- **Web-GUI Multisite** có thể sử dụng được mà không cần tới **cấu hình và Check Engine** của **Checkmk** . Đây là một giao diện web hiện đại với khả năng tải trang nhanh, cung cấp các cấu hình có thể xác định người dùng, giám sát phân tán bằng cách tích hợp nhiều thực thể giám sát thông qua **Live Status**, tích hợp **NagVis** và **PNP4Nagios**, tích hợp **LDAP**, truy cập **Status Data** thống qua các dịch vụ web và nhiều hơn nữa .
- **Multisite** sử dụng **Live Status** để truy cập vào **Status Data** .

[Tham khảo thêm](https://checkmk.com/cms_user_interface.html)
### **4) WATO - Web Administration Tool**
- Công cụ quản trị web (**WATO**) của **Checkmk** giúp cho quản trị hệ thống **Checkmk** hoàn thiện hơn bằng việc có thể truy cập hoàn toàn qua trình duyệt . 
- Công cụ này không những quản lý được Host và các dịch vụ và các rule cơ bản của **Checkmk** , mà còn bao gồm trình quản trị các User, Roles, Groups, time-periods ,....
- Với một hô hình ủy quyền hiện đại, các nhiệm vụ quản trị có thể được gán cho các user khác .
- Có thể truy cập **WATO** từ trong **Multisite** thông qua menu và các nút, cũng như tab "WATO" trên Sidebar .

    <img src=https://i.imgur.com/mch20lA.png>

[Tham khảo thêm](https://checkmk.com/cms_wato.html)
### **5) Notify**
- Hệ thống cảnh báo mới của **Checkmk** giúp việc cấu hình các cảnh báo đơn giản và linh hoạt .
- Có thể tạo nhiều kênh cảnh báo khác nhau cho mỗi người dùng .
- **VD :** Tạo 1 email báo cáo đầy đủ trong ngày, nhưng vẫn có các cảnh báo riêng biệt cho các sự kiện thông báo nghiêm trọng  .
- Các user cũng có thể tự cấu hình thông báo của mình .

[Tham khảo thêm](https://checkmk.com/cms_notifications.html)
### **6) Mobile**
- Phiên bản monile của **Multisite-GUI** được tối ưu hóa cho các smartphone và cho phép truy cập vào toàn bộ **Status Data**  .
- Phiên bản **Mobile-GUI** tự động khả dụng khi **Multisite** được cài đặt . Các thiết bị smartphone sẽ tự động được nhận dạng .
### **7) Event Console** 
- Bảng điều khiển sự kiện của **Checkmk** tích hợp quá trình xử lý log và **SNMP Traps** vào giám sát .
- Daemon riêng của nó - `mkeventd` - được cấu hình thông qua bộ **Rule Set** linh hoạt và xác định cách mà các thông báo đến được phân loại . Theo cách này, các thông báo có thể được đếm, dự đoán,...
- **Event Console** thâm chí sử dụng **Syslog-Daemon** được tích hợp sẵn để có thể nhận thông báo trực tiếp từ port `514` .

[Tham khảo thêm](https://checkmk.com/cms_ec.html)

### **8) Business Intelligence**
[Tham khảo thêm](https://checkmk.com/cms_bi.html)