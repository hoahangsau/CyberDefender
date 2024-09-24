Sau khi mở file bằng FTK Imager, mình bước vào tìm hiểu và phân tích các tệp và thư mục metadata của NTFS

![image](https://github.com/user-attachments/assets/5895b77d-c9ab-4285-9515-dd7fcd2b93b6)

`KAPE (2024-02-03T21:02:55) [NTFS]`: Đây là thư mục root chính của phân vùng NTFS được tạo bằng công cụ KAPE - một công cụ dùng để lấy và phân tích dữ liệu từ máy tính, đi kèm với đó là thời gian thư mục được tạo.

Trong thư mục root có các metadata files như trong hình:

`$BadClus`: Đây là một tệp đặc biệt thuộc NTFS, tệp này được sử dụng để phát hiện những cluster bị hỏng trong ổ đĩa, hệ điều hành sẽ tránh sử dụng những cluster này để ghi dữ liệu.

`$Extend` : là thư mục hệ thống thuộc NTFS, thư mục này chứa các tệp và metadata của hệ thống tệp. Theo như hình thì tệp này chứa các thư mục và tệp sau:

![Screenshot 2024-09-10 102707](https://github.com/user-attachments/assets/3a9a4ee2-2c76-4d44-b4d9-98dd0a4a8df7)

**$Delete** : là nơi lưu trữ tạm thời các tệp đang chờ được xóa hoàn toàn khỏi hệ thống. Những tệp này không thể bị xóa ngay lập tức do chúng đang mở (có một handle đang mở). Điều này có nghĩa là ứng dụng hoặc hệ thống vẫn đang truy cập hoặc sử dụng tệp này tại thời điểm có yêu cầu xóa.

**$ObjId** : là thư mục ẩn của NTFS để quản lý các Object ID của tệp và thư mục, trong file này có một file hệ thống con nữa tên là `$O`, file con này có chức năng lưu trữ thông tin ánh xạ của file tới ObjectID. $O chứa các GUID (Globally Unique Identifier) của các tệp và thư mục. Mỗi tệp hoặc thư mục trong hệ thống NTFS có thể có một Object ID duy nhất để theo dõi và quản lý.

**$RmMetadata\$Repair**: Thư mục này chứa thông tin về quá trình sửa lỗi, khôi phục trên hệ thống tệp sau khi phát hiện sự cố. Dữ liệu trong thư mục này giúp NTFS khôi phục tính toàn vẹn của hệ thống tệp và các tệp con, cũng như khôi phục lại cấu trúc dữ liệu nếu có sự hỏng hóc.

**$TxfLog\$Tops** : $TxfLog là thư mục lưu trữ các log liên quan đến các giao dịch của TxF trong NTFS. $Tops là một tệp con của $TxfLog và được sử dụng để lưu trữ dữ liệu đã bị ghi đè bên trong một giao dịch đang hoạt động. Khi một tệp được cập nhật trong một giao dịch, các phiên bản cũ của các trang dữ liệu được ghi vào $Tops.

**$Secure**

![Screenshot 2024-09-10 135609](https://github.com/user-attachments/assets/e5f991d6-b624-42ba-9f7e-3d4393429c97)



**$UpCase**

![image](https://github.com/user-attachments/assets/4ca55cbc-7868-4227-9f3a-3f5e2c55d8ed)

<h1>Q1</h1>
<h2>Problem</h2>
In your investigation into the FinTrust Bank breach, you found an application that was the entry point for the attack. Which application was used to download the malicious file?
<h2>Solution</h2></h2>
Đề bài hỏi suspicious file được download từ app nào nên mình tìm trong folder Download 

![image](https://github.com/user-attachments/assets/31532346-b116-4fac-ab5a-31600de75bfb)

=> Ans: Telegram
<h1>Q2</h1>
<h2>Problem</h2>
Finding out when the attack started is critical. What is the UTC timestamp for when the suspicious file was first downloaded?
<h2>Solution</h2></h2>
Trong mục Telegram Desktop mình thấy một file tên "SANS SEC401.rar", để biết được file được download vào lúc nào mình view properties của file

![image](https://github.com/user-attachments/assets/96f95063-6c7b-4607-a216-9693698644b7)

=> Ans: 2024-02-03 07:33:20

<h1>Q3</h1>
<h2>Problem</h2>
Knowing which vulnerability was exploited is key to improving security. What is the CVE identifier of the vulnerability used in this attack?
<h2>Solution</h2></h2>

Mình đã đưa file lên virustotal.com để scan và biết được câu trả lời
![image](https://github.com/user-attachments/assets/4b4e3c4a-7e22-4d70-8832-4ed12c8ff1b2)

=> Ans: CVE-2023-38831

<h1>Q4</h1>
<h2>Problem</h2>
In examining the downloaded archive, you noticed a file in with an odd extension indicating it might be malicious. What is the name of this file?
<h2>Solution</h2></h2>

<h1>Q5</h1>
<h2>Problem</h2>
Uncovering the methods of payload delivery helps in understanding the attack vectors used. What is the URL used by the attacker to download the second stage of the malware?
<h2>Solution</h2></h2>

<h1>Q6</h1>
<h2>Problem</h2>
To further understand how attackers cover their tracks, identify the script they used to tamper with the event logs. What is the script name?
<h2>Solution</h2></h2>

<h1>Q7</h1>
<h2>Problem</h2>
Knowing when unauthorized actions happened helps in understanding the attack. What is the UTC timestamp for when the script that tampered with event logs was run?
<h2>Solution</h2></h2>

<h1>Q8</h1>
<h2>Problem</h2>
We need to identify if the attacker maintained access to the machine. What is the command used by the attacker for persistence?
<h2>Solution</h2></h2>

<h1>Q9</h1>
<h2>Problem</h2>
To understand the attacker's data exfiltration strategy, we need to locate where they stored their harvested data. What is the full path of the file storing the data collected by one of the attacker's tools in preparation for data exfiltration?
<h2>Solution</h2></h2>
