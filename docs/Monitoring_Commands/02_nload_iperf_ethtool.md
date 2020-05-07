# `nload`, `iperf` và `ethtool`
- `iperf` là một công cụ miễn phí, dùng để đo lường lượng dữ liệu mạng (throughput) tối đa mà một server có thể xử lý. Công cụ này rất hữu ích để truy tìm ra các vấn đề đối với hệ thống mạng. Iperf còn có thể kiểm tra băng thông tối đa mà đường truyền mạng có thể đáp ứng.
- `nload` là một công cụ dòng lệnh để theo dõi lưu lượng mạng và việc sử dụng băng thông theo thời gian thực. Nload giúp giám sát lưu lượng đến (Incoming) và đi (Outgoing) bằng biểu đồ và cung cấp các thông tin như tổng lượng dữ liệu được truyền và mức sử dụng mạng tối thiểu và tối đa.
- `ethtool` là công cụ để hiển thị thông tin chi tiết của interface (speed, duplex,...) 
    > `ethtool` chỉ đúng với server thật hoặc server ảo chạy trên **VMware** hoặc **VirtualBox**

### **Mô hình LAB**

<img src=https://i.imgur.com/jGvK8Ca.png>

### **Thực hiện**
- 