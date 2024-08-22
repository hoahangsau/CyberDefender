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
<h1>Q6:</h1>
<h1>Q7:</h1>
<h1>Q8:</h1>

