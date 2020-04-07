# Cài đặt Checkmk
## **1) Cài đặt Checkmk trên CentOS 7**
- **B1 :** Cài đặt `epel-release` và tiện ích `wget` :
    ```
    # yum install -y epel-release wget
    ```
- **B2 :** Tải về gói cài đặt **Checkmk Raw** ( ở đây sẽ là phiên bản mới nhất - `1.6.0p11` )
    ```
    # wget https://checkmk.com/support/1.6.0p11/check-mk-raw-1.6.0p11-el7-38.x86_64.rpm
    ```
- **B3 :** Sử dụng `yum` để cài đặt gói `.rpm` để có thể tải được đầy đủ những dependencies :
    ```
    # yum install -y check-mk-raw-1.6.0p11-el7-38.x86_64.rpm
    ```
## **2) Cài đặt Checkmk trên Ubuntu Server 18.04**
## **3) Sử dụng Checkmk**
- **B1 :** Tạo site mới :
    ```
    omd create monitor
    ```
    <img src=https://i.imgur.com/1cA7zRD.png>
- **B2 :** Đổi mật khẩu cho user `cmkadmin` quản lý **Checkmk** :
    ```
    htpasswd -m /omd/sites/monitor/etc/htpasswd cmkadmin
    ```
    <img src=https://i.imgur.com/LipCgFQ.png>

    - Trong đó :
        - `/omd/sites/monitor/` là `$HOME_SITE` của site tạo password, ở đây là `monitor`
        - `cmkadmin` là tên user quản lý **Checkmk**
- **B3 :** Khởi động site :
    ```
    omd start monitor
    ```
    <img src=https://i.imgur.com/3i3zQFC.png>

- **B4 :** Cho phép port `80` trên **firewalld** (CentOS):
    ```
    firewall-cmd --permanent --add-port=80/tcp
    firewall-cmd --reload
    ```
- **B5 :** Tắt SELinux :
    ```
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    setenforce 0
    ```
- **B6 :** Trên trình duyệt của client, truy cập địa chỉ `ip-server/monitor` để kiểm tra kết quả :

    <img src=https://i.imgur.com/fz8o4lf.png>

- **B7 :** Đăng nhập với tài khoản `admin` và mật khẩu vừa khởi tạo để vào dashboard :

    <img src=https://i.imgur.com/KoFYTtf.png>