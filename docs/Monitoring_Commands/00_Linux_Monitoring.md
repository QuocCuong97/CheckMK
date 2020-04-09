# Các lệnh Monitor trong Linux
## **1) Monitor Hardwares**
### **1.1) CPU**
#### **1.1.1) `top`**
- Cú pháp :
    ```
    top [options]
    ```
    - **Options :**
        - `-v` :
        - 
- Output :

    <img src=https://i.imgur.com/LpWSCmw.png>

    - Trong đó :
        - `us` : Tỉ lệ phần trăm thời gian sử dụng CPU của user space (chạy các tiến trình do người dùng tạo ra) .
        - `sy` : Tỉ lệ phần trăm thời gian sử dụng CPU của kernel




        us: Percentage of CPU time spent in user space (running user-spawned processes).
sy: Percentage of CPU time spent in kernel space (running system processes).
ni: Percentage of CPU time spent on running processes with a user-defined priority (a specified nice value).
id: Percentage of CPU time spent idle.
wa: Percentage of CPU time spent on waiting on I/O from hardware. Example: waiting for a hard drive to finish reading data.
hi: Percentage of CPU time spent processing hardware interrupts. Example: the network card (or any piece of hardware) interrupting the CPU to notify it that new data has arrived.
si: Percentage of CPU time spent processing software interrupts. Example: a high priority service interrupting the CPU.
st: Percentage of CPU time that was stolen from a virtual machine. Example: the CPU needed to “steal” resources from a virtual machine in order to process the physical machine’s workload.