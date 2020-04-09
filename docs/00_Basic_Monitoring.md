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
    | <img src=https://i.imgur.com/S0G4U21.png width=20%> | Xanh lá | Host có thể truy cập qua mạng, đồng nghĩa với việc nó phản hồi PING |
    | <img src=https://i.imgur.com/9vHj3Cx.png width=35%> | Đỏ | Host không trả lời các truy vấn qua mạng, không thể truy cập |
    | <img src=https://i.imgur.com/ZEO46Nz.png width=50%> | Da cam | Đường dẫn đến máy chủ bị chặn, do định tuyến bị lỗi |
    | <img src=https://i.imgur.com/nMRD7yJ.png width=30%> | Xám | Host mới được thêm vào và chưa có dữ liệu giám sát |

- Đi cùng các trạng thái, một Host có các thuộc tính sau có thể được cấu hình bởi User :
    - Tên Host (duy nhất)
    - Địa chỉ IP
    - Tên phụ (tùy chọn)
    - Một hoặc nhiều **parents** (tùy chọn)
    - ...
### **2.2) Parents**
### **2.3) Services**
- Một **Host** có thể có rất nhiều dịch vụ . Dịch vụ có thể là bất cứ thứ gì .
- Trạng thái của dịch vụ chỉ có thể được xác định khi trạng thái của **Host** là `UP` .
- Một dịch vụ được giám sát có thể có các trạng thái sau :
    | Trạng thái | Màu | Ý nghĩa |
    |------------|-----|---------|
    | <img src=https://i.imgur.com/sFZE2Dd.png width=20%> | Xanh lá | Dịch vụ hoàn toàn hoạt động . Tất cả các giá trị nằm trong phạm vi cho phép |
    | <img src=https://i.imgur.com/ynbEuxo.png width=35%> | Vàng | Dịch vụ hoạt động bình thường, nhưng các thông số vượt ngoài phạm vi cho phép |
    | <img src=https://i.imgur.com/ODD3c6K.png width=30%> | Đỏ | Dịch vụ bị fail |
    | <img src=https://i.imgur.com/cQQuMJ2.png width=55%> | Da cam | Tình trạng của dịch vụ không thể được xác định chính xác . Agent cung cấp dữ liệu bị lỗi hoặc phần tử bị theo dõi đã mất |
    | <img src=https://i.imgur.com/nMRD7yJ.png width=30%> | Xám | Dịch vụ mới được thêm vào và chưa có dữ liệu giám sát |