FRS301: BlueSky Ransomware Blue Team Lab
Q1:
Để tìm ra IP của kẻ tấn công, mình sẽ vào mục Conservations để check tương tác giữa 2 endpoints. 
 ![image](https://github.com/user-attachments/assets/71e6e600-d7be-4046-a0cd-6817111c39d7)

Khi nhìn lượng packets từ IP B -> A, mình thấy tận 3033 packets nên mình đoán đây chính là IP của attacker: 87.96.21.84

Q2:
Sau khi vào EventViewer và theo dõi các event, mình thấy được tên user ở các event mà attacker cố bruteforce để login
![image](https://github.com/user-attachments/assets/81e68dda-695b-4a2d-8e77-1780997eddca)
 
=> Username: sa

Q3:
Sau khi research một chút thì mình biết được là MSSQL Sever sử dụng protocol TDS để giao tiếp, và MSSQL cũng sử dụng TDS7 (một phần của protocol TDS) để thực hiện quá trình đăng nhập của người dùng. 
	Nên mình đã filter protocol trên wireshark và tìm được thông tin username, password.
 ![image](https://github.com/user-attachments/assets/bb3014f5-11a8-41a6-bd7a-7f1a1bad2300)

=> Password: cyb3rd3f3nd3r$

Q4:
Ban đầu thì mình biết được đáp án nhờ việc research cụm từ “ MSSQL Leteral Movement” trên google
 ![image](https://github.com/user-attachments/assets/5a6ae397-9b1e-40d6-8565-3ca7bfba7337)

Nhưng để chắc chắn hơn thì mình đã vào event viewer xem lại các event. Mình thấy rằng sau khi bruteforce và login thành công, có một event mà attacker đã enable xp_cmdshell.
![image](https://github.com/user-attachments/assets/02cf6831-d5ce-4a97-875d-fbf0b59c4264)

 	Flag: xp_cmdshell
Q5:
Câu hỏi này mình cũng đã thử hỏi GPT và biết được câu trả lời 
 ![image](https://github.com/user-attachments/assets/1bd9e31d-02ee-425f-ba84-f8122d0c14b3)

Và đúng là sau khi check lại trong file event viewer, thì mình thấy attacker đã chạy chương trình winlogon.exe với hostname là MSFConsole(Metasploit-Một tool được sử dụng để pentest hệ thống)
  
  ![image](https://github.com/user-attachments/assets/4396af60-dd0d-41f1-994c-f068d6fb6391)


=>	Flag: winlogon.exe

Q6:
Khi mà download một file từ internet, giao thức được sử dụng phổ biến nhất là http. Nên mình đã export các HTTP object lits và thu được các file sau

 ![image](https://github.com/user-attachments/assets/a3758036-8783-44ac-b4b5-953318297123)


Ở packet đầu tiên mình thấy file đầu tiên được tải là checking.ps1 
=>	Flag: http://87.96.21.84/checking.ps1

Q7:
Sau khi export và mở file checking.ps1 mình thấy được ở dòng code đầu có kiểm tra nhóm người dùng được gán vào biến $priv
 ![image](https://github.com/user-attachments/assets/38562e68-33ef-4b73-a612-671ccf8522d6)

=>	Flag: S-1-5-32-544

Q8:
	Cũng ở trong cùng một file đấy, mình thấy một hàm Disable-WindowsDefender, ở biến $defenderRegistryKeys chứa những chức năng của  Window Defender bị attacker disable
 ![image](https://github.com/user-attachments/assets/3c3453a2-83c0-4b15-909d-6fd724e7aa64)

=>	Flag: DisableAntiSpyware,DisableRoutinelyTakingAction,DisableRealtimeMonitoring,SubmitSamplesConsent,SpynetReporting

Q9:
	Cũng dựa vào HTTP object list, mình biết được file được download tiếp theo là file del.ps1
	 ![image](https://github.com/user-attachments/assets/cc4a7741-2f4c-4d77-9432-7be051c2b0aa)

=>Flag: http://87.96.21.84/del.ps1

Q10:
	Trong file checking.ps1,  trong function CleanerEtc, có một dòng code được sử dụng để tạo một scheduled task
“C:\Windows\System32\schtasks.exe /f /tn "\Microsoft\Windows\MUI\LPupdate" /tr "C:\Windows\System32\cmd.exe /c powershell -ExecutionPolicy Bypass -File C:\ProgramData\del.ps1" /ru SYSTEM /sc HOURLY /mo 4 /create | Out-Null”
+ C:\Windows\System32\schtasks.exe: đây là đường dẫn đến schtasks.exe: chương trình này dùng để tạo và quản lý các scheduled tasks
+/tn "\Microsoft\Windows\MUI\LPupdate: Đây là tên của task được tạo
+/tr "C:\Windows\System32\cmd.exe /c powershell -ExecutionPolicy Bypass -File C:\ProgramData\del.ps1" /ru SYSTEM: khởi chạy một lệnh cmd.exe để thực thi tập lệnh PowerShell từ file del.ps1( -ExecutionPolicy Bypass để bypass các chính sách thực thi và thực thi các lệnh powershell)
 ![image](https://github.com/user-attachments/assets/61f83a98-3a8d-47af-90b9-376bd837f65a)

=>	Flag: \Microsoft\Windows\MUI\LPupdate

Q11:
	Sai khi export và đọc file del.ps1, mình biết được là mã độc này hoạt động bằng cách cho vô hiệu hóa các tiến trình quan trọng.
 ![image](https://github.com/user-attachments/assets/9eecbe93-5544-475d-b75d-69103daefb1b)

	Nên mình đã hỏi botChatGPT về MitreID của mã độc này 
 ![image](https://github.com/user-attachments/assets/bf0f0248-1943-4514-a057-e58edecee4ba)

=>	Flag: TA0005

Q12:
	Sau khi export toàn bộ file từ http object lists, mình thấy một file có tên rất khả nghi tên là Invoke-PowerDump.ps1, trong file này có các hàm khả nghi đề lấy thông tin dữ liệu user.
 ![image](https://github.com/user-attachments/assets/3cb9a13f-5aec-437c-8186-dcb4dbdc5319)



Q13:
	Sau khi tải file Invoke-PowerDump.ps1, attacker đã lưu file vào hashes.txt
 ![image](https://github.com/user-attachments/assets/932c6c75-beb6-466e-97c5-eee8692ee5bd)

=>	Flag: hashes.txt

Q14:
	Trong HTTP Object lists có một file extracted_hosts.txt chứa danh sách các IP bị tấn công
 ![image](https://github.com/user-attachments/assets/3597bbb3-3913-4e7d-aad1-37b626f5425a)

=>	Flag: extracted_hosts.txt

Q15:
	Sau khi phân tích malware bằng virustotal.com, mình tìm kiếm trong mục behavior và tìm được file ransom
 ![image](https://github.com/user-attachments/assets/95aadd74-9666-492e-9354-a646f1b0018e)

=>	Flag: # DECRYPT FILES BLUESKY #

Q16:
 
Sau khi phân tích malware trên virustotal.com, mình biết được họ của malware này là Conti
![image](https://github.com/user-attachments/assets/add79c98-999f-4c63-82d3-02a272e8797e)

=>	Flag: Conti 

 
 
