![image](https://github.com/user-attachments/assets/c5662a4f-a1eb-45d7-9c38-b22c3f45ab9c)THE CRIME BLUE TEAM LAB
Q1:
	Vì đề bài hỏi về trading application, chỉ cần vào check các application trong máy nạn nhân, ở đây mình dùng tool đề bài cho là ALEAPP
![image](https://github.com/user-attachments/assets/ff177d7f-8442-4c52-9909-dc315ad6ac88)

=>	FLAG: Olymp Trade

Q2:
	Đề bài nói về các cuộc gọi mà nạn nhân tránh không nghe máy và đề cập đến số tiền mà nạn nhân phải nợ. Nên mình sẽ check mục Contacts đầu tiên nhưng không tìm được thông tin nào.
![image](https://github.com/user-attachments/assets/d689cba2-72b4-4f89-a7ae-a5b2a16133fc)

	Sau đó mình chuyển qua mục discord chat để check xem nạn nhân có nhắn tin với chủ nợ không, nhưng nạn nhân không contact với chủ nợ qua discord
 ![image](https://github.com/user-attachments/assets/cb0c21b4-8847-4d19-a0c5-06abe4f6e82c)


	Cuối cùng mình check mục SMS message và thấy được message của chủ nợ gửi tới nạn nhân
 
=>	FLAG: 250000

Q3:
	Để tìm ra tên của chủ nợ, mình sẽ lần theo số điện thoại đã tìm thấy ở SMS Message và đem tìm ở mục Contact. Số điện thoại là +20 117 213 7258
 ![image](https://github.com/user-attachments/assets/e8c5303a-c870-46df-8e7e-720505d4279b)

=>	FLAG: Shady Wahab

Q4:
	Để tìm được vị trí của nạn nhân, mình nhớ ngay đến app Google map, mình vào mục Recent Activity để check thì biết được là có rất nhiều snapshot của Googlemap, Chrome, Discord được lưu lại nhằm mục đích bảo mật hệ thống. Ở trong trường hợp này, GoogleMap cũng được lưu lại một snapshot. 
![image](https://github.com/user-attachments/assets/1615bce5-2a7f-4366-8a78-2ae648457e1c)

=>	FLAG: The Nile Ritz-Carlton
Q5:
	Trước đó mình có đọc qua được đoạn hội thoại ở phần Discord Chats, nên mình nhớ ra ngay điểm đến của nạn nhân là ở The Bob Museum
 ![image](https://github.com/user-attachments/assets/7b451de5-440b-45eb-a836-c177ab1bc240)


Sau khi tra địa điểm này trên mạng, thì mình biết bảo tàng này ở Las Vegas
 ![image](https://github.com/user-attachments/assets/52e9fc4e-caf7-40e5-b68c-307172d8d9f7)


=>	FLAG: Las Vegas
Q6:
	Dựa vào cuộc hội thoại trong discord, ta biết được câu trả lời là “ The Bob Museum” 
=>	FLAG: The Bob Museum
 



