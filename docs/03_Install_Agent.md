# Cài đặt Agent trên các Host
## **Mô hình**
<img src=https://i.imgur.com/OuZOSKz.png>

## **Thực hiện**
- Trên giao diện web của **Checkmk Server**, chọn tab **Monitoring Agents** , ta sẽ nhìn thấy phần ***PACKAGED AGENTS*** chứa các gói cài đặt agent cho các host :

    <img src=https://i.imgur.com/e9vaMt9.png>

    - Ở đây chúng ta có 3 loại file cài đặt đó là :
        - `*.deb` : Dành cho các host chạy **Debian/Ubuntu** .
        - `*.rpm` : Dành cho các host chạy **RHEL/CentOS** .
        - `*.msi` : Dành cho các host sử dụng **Windows**.
        
### **1) Cài đặt Agent trên CentOS7**
