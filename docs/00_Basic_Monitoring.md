# Cơ bản về giám sát với Checkmk
## **1) States và Events**
- Hầu hết các hệ thống giám sát CNTT đều xoay quanh các **events** .
- Một **event** ( *sự kiện* ) là một thứ xảy ra duy nhất tại 1 thời điểm cụ thể ( **VD :** lỗi truy cập ổ đĩa ,...)
- Các nguồn **event** điển hình là thông báo syslog, **SNMP traps**, **Window Event Log** và các đầu vào log khác . 
- Các **event** là tự phát ( tự tạo và không đồng bộ ) .
- Ngược lại, **state** mô tả một tình huống có sự duy trì ( **VD :** Ổ đĩa đang online ) . Để quan sát trạng thái của một điều gì đó, hệ thống giám sát phải thường xuyên thăm dò nó .
- Như vậy, trong giám sát, có thể chọn làm việc với các **event** hoặc là các **state** . **Checkmk** có thể đáp ứng cả **state** và và **event**, nhưng khi có sẵn lựa chọn , luôn ưu tiên giám sát dựa trên **state** vì phương pháp này có rất nhiều lợi thế .

[Tham khảo thêm](https://checkmk.com/cms_monitoring_basics.html)

## **2) Hosts và Services**
### **2.1) Hosts**
- Mọi thứ trong **Checkmk** đều xoay quanh **Host** và **Service** .
- **Host** có thể là nhiều thứ như :
    - Một Server
    - Một thiết bị mạng ( switch, router, load balancer)
    - Một thiết bị đo có kết nối IP ( nhiệt kế, tỷ trọng kế )
    - Tất cả mọi thứ sử dụng IP
    - Một cụm máy chủ
    - Một máy ảo
    - Một Docker container
- Một Host luôn có 1 trong các trạng thái sau :
    | Trạng thái | Màu | Ý nghĩa |
    |------------|-----|---------|
    | <img src=https://i.imgur.com/S0G4U21.png width=30px> | Xanh lá | Host có thể truy cập qua mạng, đồng nghĩa với việc nó phản hồi PING |
    | <img src=https://i.imgur.com/9vHj3Cx.png width=50px> | Đỏ | Host không trả lời các truy vấn qua mạng, không thể truy cập |
    | <img src=https://i.imgur.com/ZEO46Nz.png width=70px> | Da cam | Đường dẫn đến máy chủ bị chặn, do định tuyến bị lỗi |
    | <img src=https://i.imgur.com/nMRD7yJ.png width=45px> | Xám | Host mới được thêm vào và chưa có dữ liệu giám sát |

- Đi cùng các trạng thái, một Host có các thuộc tính sau có thể được cấu hình bởi User :
    - Tên Host (duy nhất)
    - Địa chỉ IP
    - Tên phụ (tùy chọn)
    - Một hoặc nhiều **parents** (tùy chọn)
    - ...
### **2.2) Parents**
### **2.3) Services**
- Một **Host** có thể có rất nhiều dịch vụ . Dịch vụ có thể là bất cứ thứ gì .
- Trạng thái của dịch vụ chỉ có thể được xác định khi trạng thái của **Host** là <img src=https://i.imgur.com/S0G4U21.png width=22px> .
- Một dịch vụ được giám sát có thể có các trạng thái sau :

    | Trạng thái | Màu | Ý nghĩa |
    |------------|-----|---------|
    | <img src=https://i.imgur.com/sFZE2Dd.png width=30px> | Xanh lá | Dịch vụ hoàn toàn hoạt động . Tất cả các giá trị nằm trong phạm vi cho phép |
    | <img src=https://i.imgur.com/ynbEuxo.png width=50px> | Vàng | Dịch vụ hoạt động bình thường, nhưng các thông số vượt ngoài phạm vi cho phép |
    | <img src=https://i.imgur.com/ODD3c6K.png width=40px> | Đỏ | Dịch vụ bị fail |
    | <img src=https://i.imgur.com/cQQuMJ2.png width=80px> | Da cam | Tình trạng của dịch vụ không thể được xác định chính xác . Agent cung cấp dữ liệu bị lỗi hoặc phần tử bị theo dõi đã mất |
    | <img src=https://i.imgur.com/nMRD7yJ.png width=45px> | Xám | Dịch vụ mới được thêm vào và chưa có dữ liệu giám sát |

- Khi cần phải quyết định trạng thái nào tệ hơn , **Checkmk** sẽ theo thứ tự sau :
    ```
    OK -> WARN -> UNKNOWN -> CRIT
    ```
## **3) Host Group và Service Group**
- Các **host** và các **service** có thể được nhóm lại để có được cái nhìn tổng quan hơn . Theo cách này, một **host/service** có thể ở nhiều nhóm . Các nhóm này hoàn toàn là tùy chọn, không bắt buộc .
## **4) Contacts và Contact Groups**
- **Contact** và **Contact Groups** cung cấp khả năng gán user cho các **host** và **service** .
- Một **contact** có thể là 1 người dùng hoặc 1 giao diện web .
- Tuy nhiên, mối tương quan giữa **host** và các **service** không xảy ra trực tiếp mà là thông qua các **contact groups** . Theo cách này, user , cũng như các **host** và **service** có thể được gán cho nhiều **contact group** .
- User `cmkadmin` - được tạo tự động khi tạo site, luôn được quyền xem các **host** và các **service** ngay cả khi nó không phải là một **contact** . Bởi `cmkadmin` là **administrator** .
## **5) User và Roles**
- Các đặc quyền của các **users** được kiểm soát thông qua các **roles** . Mỗi **role** xác định một loạt các đặc quyền có thể tùy chỉnh :

    | Role | Ý nghĩa |
    |------|---------|
    | `admin` | Có thể xem tất cả, có tất cả các quyền |
    | `user` | Chỉ có thể xem như một **contact** . Có thể quản lý **host** trong các thư mục được gán cho họ . Không được phép thay đổi các cài đặt global |
    | `guest` | Có thể xem tất cả, nhưng không thể cấu hình bất cứ gì |

## **6) Problems, Events và Notifications**
### **6.1) Handled và unhandled problems**
- **Checkmk** sẽ nhận định các **host** không <img src=https://i.imgur.com/S0G4U21.png width=22px> , các **service** không <img src=https://i.imgur.com/sFZE2Dd.png width=22px> là các **problem** (vấn đề) .
- Một **problem** có 2 trạng thái : ***unhandled*** (chưa xử lý) và ***handled*** (đã xử lý)
- **Problem** xảy ra đầu tiên sẽ được coi là ***unhandled*** . Ngay khi có ai đó xác nhận (**acknowledge**) **problem**, , nó sẽ được chuyển sang trạng thái **handled** .
- Cũng có thể nói rằng các **unhandled problem** là các **problem** chưa có người tiếp nhận .
- Tổng quan về các **problem** sẽ được hiển thị trên thanh sidebar bên trái của dashboard :
    <p align=center><img src=https://i.imgur.com/AyFal3i.png width=50%></p>
> ***Chú ý*** : Các vấn đề về service trên một **host** không <img src=https://i.imgur.com/S0G4U21.png width=22px> sẽ không được coi là các **problems**

[Tham khảo thêm](https://checkmk.com/cms_basics_ackn.html)
### **6.2) Alert và notifications**
[Tham khảo thêm](https://checkmk.com/cms_notifications.html)
### **6.3) Flapping Hosts và Services**
- Đôi khi xảy ra 1 hiện tượng là các dịch vụ liên tục thay đổi trạng thái tại 1 thời điểm . Để tránh việc liên tục nhận được cảnh báo những lúc thế này, **Checkmk** sẽ chuyển dịch vụ sang trạng thái ***flapping*** . Điều này được minh họa bằng biểu tượng <img src=https://i.imgur.com/nw7xE7Y.png width=15px> .
- Khi một dịch vụ chuyển sang trạng thái ***flapping*** , một thông báo sẽ được gửi đến người dùng về sự thay đổi này, sau đó bật chế độ im lặng cho các cảnh báo sau . Sau một thời gian thích hợp, nếu không có tình trạng thay đổi trạng thái nhanh nữa, và kết quả cuối cùng của dịch vụ là tốt hoặc xấu thì trạng thái ***flapping*** sẽ biến mất và các cảnh báo  bình thường trở lại .
### **6.4) Scheduled Downtimes**
- Nếu phải thực hiện việc bảo trì Server, thiết bị hay phần mềm, bình thường người quản trị sẽ muốn tránh đi các cảnh báo sự cố phiền hà trong thời gian này . Ngoài ra, họ cũng có thể muốn cho các đồng nghiệp khác biết rằng các vấn đề xuất hiện trong thời gian này có thể tạm thời bỏ qua .
- Đối với trường hợp này, người quản trị có thể nhập vào một điều kiện về lịch **downtimes** cho **host** hoặc **services** . Điều này có thể được thực hiện ngay khi bắt đầu bảo trì hoặc trước đó hẳn một thời gian . **Scheduled downtimes** được minh họa qua các biểu tượng :
    - <img src=https://i.imgur.com/DA35dN5.png width=20px> : **Host/Service** đang trong thời gian **downtimes**
    - <img src=https://i.imgur.com/ymrHcOM.png width=20px> : **Host** có chứa **service** đang trong thời gian **downtimes**
- Khi một **Host/Service** ở trong thời gian **downtimes** :
    - Sẽ không có thông báo được gửi đến
    - Các **problem** sẽ không được hiển thị trên bảng **Tactical Overview**
## **7) Timeperiods**
- **Timeperiods** định nghĩa các khoảng thời gian thường xuyên, định kỳ hàng tuần được sử dụng cho các vị trí các nhau trong cấu hình giám sát .
- Một **timeperiod** điển hình là giờ làm việc (**workhours**) thường nhận thời gian từ `8:00 - 17:00` tất cả các ngày trong tuần trừ thứ 7 và Chủ nhật . **Timeperiod** cũng có thể bao gồm một số khoảng thời gian ngoại lê - như ngày Tết, Quốc Khánh,...
- Một số tình huống thường dùng tới **time period** :
    - Giới hạn thời gian nhận thông báo (***notification period***)
    - Giới hạn thời gian các lần kiểm tra được thực hiện (***check period***)
    - Thời gian phục vụ cho việc đánh giá tính khả dụng hệ thống (***service period***)
    - Thời gian trong khi **event console** áp dụng các rules .
## **8) Check interval, check attempts và check period**
## **9) Active và Passive Checks**
## **10) Tổng quan về các icon quan trọng của Host và Service**
- Bảng sau đây cung cấp một cái nhìn vắn tắt về các icon trạng thái quan trọng xuất hiện bên cạnh **Host** và **Service** :

    | Icon | <div align=center>Ý nghĩa</div> |
    |------|---------|
    | <img src=https://i.imgur.com/DA35dN5.png width=20px> | **Host/Service** đang trong thời gian **downtimes** |
    | <img src=https://i.imgur.com/ymrHcOM.png width=20px> | **Host** có chứa **service** đang trong thời gian **downtimes** |
    | <img src=https://i.imgur.com/eCtQxUD.png width=20px> | **Host/Service** đang ở ngoài khoảng thời gian nhận thông báo (***notification period***) |
    | <img src=https://i.imgur.com/3rbjY1D.png width=20px> | Thông báo rằng **Host/Service** đang bị deactivated |
    | <img src=https://i.imgur.com/IG1x8GP.png width=20px> | Checks cho **Service** đang bị vô hiệu hóa |
    | <img src=https://i.imgur.com/jaVtA6F.png width=20px> | **Host/Service** có trạng thái ***stale*** |
    | <img src=https://i.imgur.com/nw7xE7Y.png width=20px> | **Host/Service** đang ở trạng thái ***flapping*** |
    | <img src=https://i.imgur.com/PQ98MZI.png width=20px> | **Host/Service** có **problem** đã được nhận |
    | <img src=https://i.imgur.com/AXF30JO.png width=20px> | Có một comment cho **Host/Service** |
    | <img src=https://i.imgur.com/QGRvBb9.png width=20px> | **Host/Service** là một phần của hệ thống **BI** |
    | <img src=https://i.imgur.com/9Ma3p6x.png width=20px> | Tại đây có thể truy cập trực tiếp các cài đặt của check |
    | <img src=https://i.imgur.com/INGH0uy.png width=20px> | Chỉ dành cho dịch vụ **logwatch** : tại đây có thể truy cập các file log |
    | <img src=https://i.imgur.com/zljsWtd.png width=20px> | Tại đây bạn có thể truy cập biểu đồ thời gian của hiệu suất dữ liệu |
    | <img src=https://i.imgur.com/KypdT8i.png width=20px> | **Host/Service** có dữ liệu tồn. Click để xem chi tiết |
    | <img src=https://i.imgur.com/fbuPwkR.png width=20px> | Kiểm tra bị lỗi. Click để xem và gửi báo cáo sự cố/lỗi |
    