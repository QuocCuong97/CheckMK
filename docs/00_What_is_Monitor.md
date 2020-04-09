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

<span style="color:blue">some *This is Blue italic.* text</span>

**My Bold Text, in red color.**{: style="color: red; opacity: 0.80;" }


$\color{#0b3}{your-text-here}$