# `stress`
- `stress` là một câu lệnh được sử dụng để áp một mức tải cho hệ thống để kiểm tra khả năng chịu tải của nó trong thực tế. Nó sẽ tạo ra các process sử dụng rất nhiều tài nguyên của hệ thống như CPU, RAM hay disk để ta có thể kiểm tra được khả năng và ngưỡng mà hệ thống của ta hoạt động tốt nhất. Nó cũng rất có ích để tái hiện lại trạng thái tải cao gây ra lỗi hệ thống để ta có thể tìm cách xử lý.
### **Cài đặt**
    ```
    yum install stress -y     (RHEL/CentOS)
    apt install stress -y     (Ubuntu/Debian)
    ```
---
### **Đẩy tải CPU : sử dụng option `-c` hoặc `--cpu`**
- Trước khi sử dụng lệnh `stress` :
        
    <img src=https://i.imgur.com/YTF7Klj.png>

- Thực hiện lệnh đẩy tải 1 CPU :
    ```
    stress -c 1
    ```
    <img src=https://i.imgur.com/FV8nitZ.png>

- Thực hiện đẩy tải 2 CPU ( nếu có )
    ```
    stress -c 2
    ```
    <img src=https://i.imgur.com/nm1FAdC.png>

- Giới hạn thời gian cho lệnh `stress` : option `-t` hoặc `--timeout` ( tính theo giây )
    ```
    stress -c 2 --timeout 600
    ```
-------
### **Đẩy tải RAM : sử dụng option `-m` hoặc `--vm`**
- Trước khi sử dụng lệnh `stress` :

    <img src=https://i.imgur.com/oAjT2GU.png>

- Thực hiện đẩy tải thêm 2GB RAM :
    ```
    stress -m 1
    ```
    <img src=https://i.imgur.com/AQiHFk6.png>

    > Câu lệnh này sẽ sử dụng hàm `malloc()` để cấp phát vùng nhớ cho 1 process ở đây sẽ cấp là `256M`.
- Chỉ ra dung lượng cấp cho process :
    ```
    stress -m 1 --vm-bytes 500M
    ```
    <img src=https://i.imgur.com/YBY6ODm.png>

- Nếu đẩy tải quá giới hạn của RAM, RAM sẽ chuyển bớt cho swap :
    ```
    stress -m 1 --vm-bytes 2.5G
    ```
    <img src=https://i.imgur.com/zHHdD5b.png>
- Giới hạn thời gian cho lệnh `stress` : option `-t` hoặc `--timeout` ( tính theo giây )
    ```
    stress -m 2 --timeout 600
    ```
> **Lưu ý :** CPU sẽ bị đẩy tải theo khi đẩy tải RAM
------
### **Đẩy tải Disk : sử dụng option `-i` hoặc `--io`**
- Thực hiện lệnh đẩy `I/O` :
    ```
    stress -i 1
    ```
    > Câu lệnh này sẽ gọi đến hàm `sync()`
> **Lưu ý :** CPU sẽ bị đẩy tải theo khi đẩy tải Disk
--------
### **Ghi dữ liệu tạm vào Disk : sử dụng option `-d` hoặc `--hdd`**
- Trước khi thực hiện lệnh `stress` :

    <img src=https://i.imgur.com/tnczrDB.png>

- Thực hiện ghi dữ liệu tạm `2G` vào disk :
    ```
    stress -d 1 --hdd-bytes 2G
    ```
    <img src=https://i.imgur.com/ceh9dri.png>

> **Lưu ý :** CPU sẽ bị đẩy tải theo khi đẩy tải Disk