<img src=https://i.imgur.com/C12xNcg.jpg>

# Open Ports
## **Đặt vấn đề**
- Scan **port** là một trong các task thú vị nhất để xử lý bất khi nào cần thu thập dữ liệu **OSINT** về một mục tiêu từ xa . Tuy nhiên, các **port** được mở không chỉ quan trọng với hướng tấn công, mà còn quan trọng với cả hướng phòng thủ .
- Tưởng tượng rằng sếp của bạn giao cho bạn trách nhiệm cài đặt **Firewall** để lọc lưu lượng truy cập - nhưng bạn không hiểu đầy đủ về cách mà các **port** đang mở hoạt động , tại sao chúng lại mở , và **port** nào thì cần lọc .
- **Firewall** là một công cụ tuyệt vời, nhưng nó sẽ không giúp bạn cấu hình toàn bộ các quy tắc lọc ( ***filter rules*** ) ( một số thì có, nhưng chúng sẽ không thường xuyên đề xuất các cấu hình chính xác và an toàn nhất ), cũng sẽ không khai sáng cho bạn tất cả các nhu cầu bảo mật của các dịch vụ hệ thống đang chạy .
- Khi bạn đọc qua các blog về kỹ thuật, các hướng dẫn sử dụng và các tài nguyên khác liên quan đến scan **port** và **port** mở, bạn sẽ nhận thấy nhiều đề xuất về việc đóng các **port** đang mở. Nhưng những gợi ý đó đến từ đâu? Có thực sự cần thiết phải đóng các **port** đang mở? Có phải tất cả các cổng đang mở đều đang chứa các lỗ hổng bảo mật ? Hãy cùng tìm hiểu!
## **Port mở là gì?**
- Bất cứ khi nào nói đến "**port**", người ta có thể nghĩ tới 2 khái niệm : 
    - Lỗ nằm mặt sau hoặc mặt bên của bất kỳ các thiết bị mạng nào, chẳng hạn như cổng Ethernet ,...
    - Khi liên quan đến IP và các dịch vụ Internet, **port** lại là các cổng ảo có thể mở, hoặc đóng .
- Server bao gồm rất nhiều dịch vụ đang chạy ; VD : HTTP Server cho phép bạn duyệt trang `google.com`, hoặc một Mail Server cho phép người dùng gửi và nhận email . Mặt khác, các nhà phát triển sử dụng các giao thức truyền file như `FTPS` hay `SSH` để chạy các đường hầm mã hóa trên các máy tính sử dụng để chia sẻ thông tin với nhau .
- Đây chính là khái niệm "**port**" . Một "**port**" cơ bản là một cách giúp các hệ thống xác định, thiết lập và truyền dữ liệu từ bên này sang bên kia .
- Các **port** được chỉ định bởi các số . Chẳng hạn, nếu 1 **port** được mở (được sử dụng), nó có thể lắng nghe bất kỳ số nào trong dải `1-65535` . Đúng vậy, có `65535` **port** có thể được gắn cho bất kỳ dịch vụ nào .
- Và khi bạn cài đặt một hệ điều hành trên PC hoặc trên các máy ảo, sau đó giả sử bạn cài đặt thêm các dịch vụ như **NGINX** hay **Apache** thfi chúng sẽ cần một **port** để `daemon` (dịch vụ) sẵn sàng nhận và gửi dữ liệu qua mạng .
- Khi 1 **port** đang chạy với 1 số cụ thể , bạn sẽ không thể chạy dịch vụ nào khác trên cùng 1 **port** đó . VD : khởi động dịch vụ **Apache** sau khi đã cài đặt **NGINX** trên **port** `80` sẽ dẫn đến việc khởi động không thành công bởi vì **port** đang được sử dụng .
- Có một điều hiển nhiên rằng, bất cứ khi nào duyệt web, bạn sẽ kết nối tới **port** `80` (đối với `HTTP`) hoặc `443` (đối với các yêu cầu dựa trên `HTTPS`) hoặc bất cứ khi nào thực hiện `SSH`, thông tin sẽ đi qua **port** `22` .
- Như bạn đã thấy, các **port** mở giúp các thiết bị mạng và hệ điều hành giao tiếp với nhau và truyền thông tin chính xác theo cách thích hợp nhất .
- Các **port** đóng, mặt khác, chỉ là các **port** không còn dịch vụ nào lắng nghe trên chúng . Nếu bạn thực hiện lệnh `service nginx stop`, bạn sẽ đóng tất cả các kết nối đã thiết lập, và không một ai có thể truy cập trang web của bạn cho đến khi `start` lại nó .
## **Vậy các **port** mở có nguy hiểm không?**
- Có một quan niệm sai lầm phổ biến rằng các **port** mở là nguy hiểm. Rất nhiều các phương tiện truyền thông khuyên rằng bạn nên đóng các **port**, và trong khi điều này có thể phù hợp trong 1 số ngữ cảnh, thì nó cũng không thể nói chính xác rằng để các **port** mở là nguy hiểm .
- Các **port** mở đã ở đó khi có sự xuất hiện của Internet. Xét cho cùng, chúng là những cánh cổng giúp bạn liên lạc với đồng nghiệp, bạn bè và gia đình trên toàn bộ các dịch vụ mạng .
- Vì vậy, thay vì nó thẳng ra rằng các **port** mở là nguy hiểm, chúng ta các thể nói đơn giản rằng các quy tắc bảo mật (***security rules***) server và mạng bị cấu hình sai, cùng với các bản phần mềm chưa được vá (***unpatched***) và dễ bị tấn công (***vulnerable***) , tất cả chúng dẫn đến việc các công ty phải khai thác các dịch vụ chỉ trên một số **port** nhất định .
- Điều này là vì các **port** mở mặc định không nguy hiểm . Đó là những gì chúng ta làm trên các **port** mở ở mức hệ thống, những gì ta thực hiện với các dịch vụ và ứng dụng chạy trên các **port** này . Điều này nhắc cho mọi người rằng liệu có nên gán nhãn cho chúng là "**nguy hiểm**" hay không .
## **Các **port** mở phổ biến**
- Có rất nhiều các phần mềm scan **port** quanh chúng ta, một số được xây dựng cho các task cụ thể , một số khác được bao gồm trong các công cụ quét lỗ hổng trực tuyến . Cho dù sử dụng chúng thế nào, cần biết rằng "scan" là điều bắt buộc để tìm ra các **port** đang mở .
- Sau đây là một danh sách tóm tắt ngắn gọn về các **port** được mở phổ biên nhất hiện nay sau khi thu thập thông tin trên nhiều máy :
    - `FTP - 20,21` : là các **port** được sử dụng trong kết nối `FTP` giữa client và server
    - `SSH - 22` : là **port** OpenSSH Server được sử dụng theo mặc định trên hầu hết các máy Unix/Linux .
    - `Telnet - 23` : dành riêng cho ứng dụng `Telnet` server nhận kết nối từ bất cứ telnet client nào .
    - `SMTP - 25` : được dành riêng để chuyển các messages giữa các **MTAs** - **Mail Transfer Agents** .
    - `DNS - 53` : là nơi DNS Server chạy và một trong những daemon nổi tiếng sử dụng **port** này là `bind` .
    - `DHCP - 67, 68` : **port** `67` được sử dụng cho DHCP Server, và **port** `68/UDP` được sử dụng cho DHCP Client .
    - `HTTP - 80` : là **port** được gán cho Web Server và được liên kết với **HyperText Transfer Protocol** .
    - `POP3 - 110` : là **Post Office Protocol**, một trong các giao thức truyền thống nhất được sử dụng bởi các ứng dụng email để lấy dữ liệu từ các Email Server đầu xa .
    - `IMAP - 143` : là **port** **IMAP** mặc định cho các kết nối không được mã hóa .
    - `HTTPS - 443` : là **port** được sử dụng để phục vụ tất cả các yêu cầu dựa trên SSL của bất kì trang web nào .
## **Các **port** mở mặc định**
- Điều này phụ thuộc vào hệ điều hành bạn đang chạy, vì không phải hệ điều hành nào cũng chạy các dịch vụ giống nhau.
- **VD :** Windows, MAC OS, Linux đều chạy các core daemons khác nhau, do đó cổng mở trên HĐH này có thể bị đóng trên HĐH kia .
- Danh sách các cổng mở mặc định phụ thuộc rất lớn vào HĐH, phiên bản hoặc distribution . 
- **VD :** Các trang web MAC OS nói rằng có khoảng `132` **port** có khả năng được sử dụng trên các dịch vụ của **Apple** .
- Trong thử nghiệm sau đây , chúng ta chạy quét `Nmap` trên một **Ubuntu Image** để xác định các **port** mở mặc định : 
    ```
    root@securitytrails-ubuntu:~# nmap -p 1-65535 localhost
    Starting Nmap 7.60 ( https://nmap.org ) at 2019-12-03 19:11 UTC
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000030s latency).
    Not shown: 65534 closed ports
    PORT STATE SERVICE
    22/tcp open ssh

    Nmap done: 1 IP address (1 host up) scanned in 6.54 seconds
    root@securitytrails-ubuntu:~#
    ```
    - Như chúng ta thấy bên trên, mặc định tất cả các **Ubuntu** images của **DigitalOcean** sẽ chỉ để mở **port** `22`  - tất nhiên, để người dùng có cách mà kết nối đến Server .
- Tuy nhiên trên **CentOS**, số port mở mặc định lại là `3` :
    ```
    [root@securitytrails-centos:~]# nmap -p 1-65535 localhost
    Starting Nmap 6.40 ( https://nmap.org ) at 2019-12-03 19:16 UTC
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.0000040s latency).
    Other addresses for localhost (not scanned): 127.0.0.1
    rDNS record for 127.0.0.1: securitytrails-centos
    Not shown: 65534 closed ports
    PORT    STATE SERVICE
    22/tcp  open  ssh
    25/tcp  open  smtp
    111/tcp open  rpcbind

    Nmap done: 1 IP address (1 host up) scanned in 0.86 seconds
    [root@securitytrails-centos:~]#
    ```
## **Cách tìm kiếm các port đang mở**
- Công cụ scan **port** phổ biến nhất luôn là `Nmap` .
- Để scan một máy local, có thể thực hiện bằng lệnh sau :
    ```
    nmap -p 1-65535 localhost
    ```
- Để scan một máy remote, thực hiện lệnh sau :
    ```
    nmap -p 1-1024 X.X.X.X
    ```
    > Trong đó `X.X.X.X` là địa chỉ IP máy remote .
- Từ `1 -> 65535`, có thể scan bất cứ **port** nào bạn cần, tuy nhiên phạm vi càng rộng thời gian scan càng lâu .
- Nếu muốn scan các **port** phổ biến nhất, có thể sử dụng lệnh :
    ```
    nmap --top-ports 20 X.X.X.X
    ```
    > Trong đó `20` là số lượng **port** phổ biến muốn scan
    - Output :
        ```
        [root@centos7-01 ~]# nmap --top-ports 20 localhost

        Starting Nmap 6.40 ( http://nmap.org ) at 2020-04-11 02:01 +07
        Nmap scan report for localhost (127.0.0.1)
        Host is up (0.000064s latency).
        Other addresses for localhost (not scanned): 127.0.0.1
        PORT     STATE  SERVICE
        21/tcp   closed ftp
        22/tcp   open   ssh
        23/tcp   closed telnet
        25/tcp   open   smtp
        53/tcp   closed domain
        80/tcp   closed http
        110/tcp  closed pop3
        111/tcp  closed rpcbind
        135/tcp  closed msrpc
        139/tcp  closed netbios-ssn
        143/tcp  closed imap
        443/tcp  closed https
        445/tcp  closed microsoft-ds
        993/tcp  closed imaps
        995/tcp  closed pop3s
        1723/tcp closed pptp
        3306/tcp closed mysql
        3389/tcp closed ms-wbt-server
        5900/tcp closed vnc
        8080/tcp closed http-proxy

        Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds
        ```
## **Tổng kết** 
- Các **port** sẽ luôn là các cánh cửa đầu tiên bị tấn công bởi các hacker. Nếu mở, chúng có thể trở thành các mỗi đe dọa thật sự nếu các dịch vụ quan trọng đang chạy trên nó .
- Nếu bạn làm việc cho một công ty dựa trên web, việc xem xét các **port** được mở luôn là việc được ưu tiên hàng đầu. Điều này mang lại một khởi đầu tốt hơn, giúp giảm thiểu được phần nào rủi ro từ các cuộc tấn công mạng .

-------------------------
Nguồn : https://securitytrails.com/blog/open-ports
