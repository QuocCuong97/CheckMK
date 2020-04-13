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
- **B1 :** Cài đặt `xinetd` :
    ```
    # yum install -y xinetd
    ```
- **B2 :** Khởi động `xinetd` và cấu hình khởi động dịch vụ cùng hệ thống :
    ```
    # systemctl start xinetd
    # systemctl enable xinetd
    ```
- **B3 :** Copy đường dẫn file `check-mk-agent-1.6.0p11-1.noarch.rpm` trên **Checkmk Server** rồi tải xuống trên host :
    ```
    # yum install -y wget
    # wget http://192.168.5.20/monitor/check_mk/agents/check-mk-agent-1.6.0p11-1_.noarch_.rpm
    ```
    > Trong đó : `192.168.5.20` là IP của **Checkmk Server**
- **B4 :** Cài đặt agent :
    ```
    # rpm -ivh check-mk-agent-1.6.0p11-1.noarch.rpm
    ```
- **B5 :** Sửa file cấu hình `xinetd` của host tại `/etc/xinetd.d/check_mk` :
    ```
    # vi /etc/xinetd.d/check_mk
    ```
    - Tại dòng `44`, bỏ dấu `#` ở đầu dòng và thêm vào IP của **Checkmk Server** :

        <img src=https://i.imgur.com/a3Zl4rw.png>

- **B6 :** Khởi động lại dịch vụ `xinetd` :
    ```
    # systemctl restart xinetd
    ```
- **B7 :** Cho phép port `6556` của checkmk đi qua **Firewalld** :
    ```
    # firewall-cmd --permanent --add-port=6556/tcp
    # firewall-cmd --reload
    ```
### **2) Cài đặt Agent trên Ubuntu Server 18.04**
- **B1 :** Cài đặt `xinetd` :
    ```
    # apt-get install -y xinetd
    ```
- **B2 :** Khởi động `xinetd` và cấu hình khởi động dịch vụ cùng hệ thống :
    ```
    # systemctl start xinetd
    # systemctl enable xinetd
    ```
- **B3 :** Copy đường dẫn file `check-mk-agent_1.6.0p11-1_all.deb` trên **Checkmk Server** rồi tải xuống trên host :
    ```
    # wget http://192.168.5.20/monitor/check_mk/agents/check-mk-agent_1.6.0p11-1_all.deb
    ```
    > Trong đó : `192.168.5.20` là IP của **Checkmk Server**
- **B4 :** Cài đặt agent :
    ```
    # dpkg -i check-mk-agent_1.6.0p11-1_all.deb
    ```
- **B5 :** Sửa file cấu hình `xinetd` của host tại `/etc/xinetd.d/check_mk` :
    ```
    # vi /etc/xinetd.d/check_mk
    ```
    - Tại dòng `44`, bỏ dấu `#` ở đầu dòng và thêm vào IP của **Checkmk Server** :

        <img src=https://i.imgur.com/prXo4LW.png>

- **B6 :** Khởi động lại dịch vụ `xinetd` :
    ```
    # systemctl restart xinetd
    ```
### **3) Cài đặt Agent trên Windows Server 2016**
- **B1 :** Paste đường dẫn sau vào trình duyệt để download Agent (nên dùng Chrome) :
    ```
    http://192.168.5.20/monitor/check_mk/agents/windows/check_mk_agent.msi
    ```
    > Trong đó : `192.168.5.20` là IP của **Checkmk Server**
- **B2 :** Run file `check_mk_agent.msi`, chọn ***Next*** :
    
    <p align=center><img src=https://i.imgur.com/SpRzPAe.png width=50%></p>

- **B3 :** Tại cửa sổ **End-User License Agreement**, chọn ***I accept the terms in the License Agreement***, rồi chọn ***Next*** :

    <p align=center><img src=https://i.imgur.com/ICs1jrH.png width=50%></p>

- **B4 :** Tại cửa sổ **Destination Folder**, chọn ***Install and start service*** , rồi chọn ***Next*** :

    <p align=center><img src=https://i.imgur.com/iMhYXAX.png width=50%></p>

- **B5 :** Tại cửa sổ **Ready to install Check MK Agent 1.6**, chọn ***Install*** :

    <p align=center><img src=https://i.imgur.com/g2IzWqN.png width=50%></p>

- **B6 :** Chọn ***Finish*** sau khi hoàn tất cài đặt :

    <p align=center><img src=https://i.imgur.com/JIifsuV.png width=50%></p>

### **4) Thêm host vào Checkmk Server**
- **B1 :** Trên **Checkmk server**, tại menu **WATO - Configuration** chọn ***Hosts*** :

    <div align=center><img src=https://i.imgur.com/j8fTxUn.png width=40%></div>

- **B2 :** Chọn ***Create new host*** :

    <img src=https://i.imgur.com/TJAmyI4.png>

- **B3 :** Thêm các thông tin của host, sau đó chọn ***Save & Test*** :

    <img src=https://i.imgur.com/e2kBbb8.png>

- **B4 :** Sau khi cửa sổ **Ping** và cửa sổ **Agent** đều test OK và hiện màu xanh, chọn ***Save & Exit*** để quay về màn hình trước :

    <img src=https://i.imgur.com/OwvcZBh.png>

- **B5 :** Chọn ***Save & go to Service*** :

    <img src=https://i.imgur.com/8fe8Gtf.png>

- **B6 :** Chọn ***Fix all missing*** để khi lưu lại thì các dịch vụ sẽ lên luôn các thông số : 

    <img src=https://i.imgur.com/lN2QtBC.png>

- **B7 :** Chọn Tab ***Changes*** để xem các thay đổi vừa tạo :

    <img src=https://i.imgur.com/U0JCPBT.png>

- **B8 :** Chọn ***Activate affected*** để áp dụng các thay đổi :

    <img src=https://i.imgur.com/QZz6mUL.png>

- **B9 :** Kiểm tra lại bằng cách vào menu **VIEWS** -> **Hosts** -> **All hosts** , ta sẽ thấy host vừa thêm : 

    <img src=https://i.imgur.com/gSlvLLc.png>

    - Làm tương tự với các host còn lại (đã cài agent) :

        <img src=https://i.imgur.com/RFZ6vmP.png>
---------------------------------
## Tham khảo
- https://checkmk.com/cms_wato_monitoringagents.html
- https://checkmk.com/cms_agent_linux.html
- https://checkmk.com/cms_agent_windows.html