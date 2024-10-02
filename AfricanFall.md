**FRS301-IA1801-Phan Thuận Hóa**


Q1:
	Sau khi tải file lab về mình có ngó qua file DiskDrigger.ad1.txt, sau khi lướt xuống dưới thì mình thấy MD5 checksum
  ![image](https://github.com/user-attachments/assets/8a699523-9a52-4010-9a22-192856c71b43)
 
=>	Flag: 9471e69c95d8909ae60ddff30d50ffa1


Q2:
	Sau khi mở file DiskDrigger.ad1 bằng FTK Imager, vì đề bài bảo là tìm ra “suspect search for on 2021-04-29 18:17:38 UTC” nên mình đã cố tìm các file chứa History của trình duyệt search
 ![image](https://github.com/user-attachments/assets/ccfe8377-9b70-4d64-bdb2-679d628831aa)

Vì để bài cho thời gian là 29-04-2021 nên mình biết ngay là phải tìm History trong file Google/Chrome/UserData/Default/, sau khi view file đấy trong 
DB Browser for SQL Lite, mình hỏi bot chat GPT thì mình biết được là mình có thể tìm lịch sử search theo thời gian trong table urls hoặc keyword_serach_terms
 ![image](https://github.com/user-attachments/assets/f2f0ca01-d351-408f-8998-409160e5ab21)

Và mình quyết định tìm trong table urls vì trong đó có cột “last_visit_time”
Sau khi research thì mình thấy là cột last_visit_time này thể hiện định dạng số micro giây kể từ epoch (điểm bắt đầu của hệ thống thời gian trong chrome). Nên mình đã nhờ BotchatGPT convert “2021-04-29 18:17:38 UTC” sang định dạng đó rồi filter trên database.
 
=>	Flag: password cracking lists


Q3:
	Đề bài có nhắc tới FTP(File Transfer Protocol), mình nhớ ngay đến FileZilla(là một phần mềm kết nối FTP), sau khi tìm sâu vào trong thư mục Roaming, mình tìm thấy file recentsevers.xml, file này chứa các thông tin cơ bản về các máy chủ FTP mà user đã kết nối từ trước. 
 
![image](https://github.com/user-attachments/assets/4ed4fdef-0b06-44c9-8d8b-c7032a2f6e92)

=>	Flag: 192.168.1.20


Q4:
	Để xem thông tin của một file deleted, mình vào mục Recycle Bin và tìm thấy câu trả lời ở cột Date Modified của tệp $IW9BJ2Z.txt

 ![image](https://github.com/user-attachments/assets/e03ccd7c-c3f3-4923-8c91-5f89daf9c350)

=>	Flag: 2021-04-29 18:22:17 UTC


Q5:
	Sau một hồi research thì mình biết được là khi mình chạy một ứng dụng trên hệ điều hành Windows lần đầu tiên, Windows sẽ tạo một tập tin prefetch tương ứng để ghi nhớ thông tin về việc khởi động ứng dụng đó. Nên mình đã truy cập vào file Prefetch để tìm thông tin về TOR
 
![image](https://github.com/user-attachments/assets/5467e945-ee47-4843-b3a6-42c65ae6d94f)

Sau khi lướt xuống thì mình chỉ tìm thấy 2 file ghi lại event trình duyệt TOR được cài đặt chứ chưa hề được chạy.

=>	Flag: 0 

Q6: 

Để tìm được email, mình đoán là user sẽ đăng nhập email qua trình duyệt nên mình view lại file HISTORY đã tìm được ở trên và filter với cụm “@” và tìm được kết quả:

 
![image](https://github.com/user-attachments/assets/b9961a27-8003-41f9-9613-a953b2209b97)


=>	dreammaker82@protonmail.com

Q7: 
FQDN: Tên miền đầy đủ điều kiện
Đến câu này thì mình phải đọc wup từ trên mạng thì mới có thể làm được do kiến thức có hạn. Mình lần theo đường dẫn “C:\Users\JohnDoe\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\”: 	File PSReadLine này sẽ hiển thị các câu lệnh đã nhập của user trong powershell
 ![image](https://github.com/user-attachments/assets/1a4dcaf7-3fa8-4914-aa3b-73d9161f8d11)


Lướt xuống dưới mình thấy command “nmap dfir.science” 
=>	Flag: dfir.science

Q8: 

Mình tìm thấy file trong mục Pictures\Contact\
 
![image](https://github.com/user-attachments/assets/62ef9d4b-ed75-4252-b105-519559e25856)


Sau khi dùng exiftool để phân tích thì mình thấy được kinh độ và vĩ độ của nơi bức ảnh được chụp.
 ![image](https://github.com/user-attachments/assets/968b17ff-f3c9-4d76-bb2d-4ea48207a7ac)


Sau đó mình tìm trên google map bằng cách nhập 16°00'00.0"S 23°00'00.0"E  
![image](https://github.com/user-attachments/assets/d5d344da-fe6d-423f-b2a8-ba4fe91f7092)

=>	Flag: ZAMBIA
Q9:
Sau khi phân tích ảnh bằng exiftool, mình biết được bức ảnh được chụp bằng điện thoại LG
 ![image](https://github.com/user-attachments/assets/29221887-61e8-4817-b416-0a562ca861f1)

Trong quá trình research, mình biết được ảnh chụp bằng camera được lưu ở /DICM/Camera

=>	Flag: Camera
Q10: 
Sau khi đưa lên ChatGPT phân tích hash
"Anon:1001:aad3b435b51404eeaad3b435b51404ee:3DE1A36F6DDB8E036DFD75E8E20C4AF4:::"
 thì mình biết được 
“aad3b435b51404eeaad3b435b51404ee” là hash LM , còn “3DE1A36F6DDB8E036DFD75E8E20C4AF4:::” là hash NT
![image](https://github.com/user-attachments/assets/40170a3c-9b25-450a-b218-9c18e3dad02a)

 
Nên mình sử dụng tool online hashes.com để decrypt 
 ![image](https://github.com/user-attachments/assets/676d4484-7b22-4854-b344-e93a2cd953b7)


=>	Flag: AFR1CA!




Q11: 
Sau khi đọc hint và research thì mình biết được là trong window, user’s passwords được giữ trong file registry SAM và SYSTEM
•	C:/Windows/System32/config/SAM
•	C:/Windows/System32/config/SYSTEM

Nên mình đã export hai file này ra Downloads
 ![image](https://github.com/user-attachments/assets/b5c752a8-616c-4001-969c-b313d7baafec)


Sau khi sử dụng mimikatz để trích xuất mật khẩu, mình ra được đoạn hash NTLM của user John Doe là “ecf53750b76cc9a62057ca85ff4c850e”
 ![image](https://github.com/user-attachments/assets/ca9c13b8-4a05-4b59-a12f-914455c65188)



Sau khi decrypt trên hashes.com thì mình đã ra được mật khẩu
 

=>FLAG: ctf2021


 
 
