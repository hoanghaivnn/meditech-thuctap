# Cung cấp key cho người dùng
## Tạo khóa RSA bằng lệnh 

```ssh-keygen -t rsa -b 1024```

Tại đây ta đặt tên file và mật khẩu cho người thứ nhất muốn truy cập vào máy chủ. Ví dụ ở đây mình đặt tên file là meditech và mật khẩu truy cập file là meditech

<img src="http://i.imgur.com/X5rLthO.png">

và cấp quyền cho 2 file 

<img src ="http://i.imgur.com/4uxo0fF.png">

Sau đó copy file meditech ra ngoài máy that (dùng winSPC)
**SCP là gì?**

- SCP (Secure Copy – Sao chép an toàn) là một ứng dụng sử dụng SSH để mã hóa toàn bộ quá trình chuyển tập tin.
- SCP là lệnh dùng để di chuyển file dữ liệu giữa các máy tính chạy hệ điều hành Linux từ xa chỉ cần biết địa chỉ ip
- SCP dùng ssh để di chuyển dữ liệu, có chế độ bảo mật giống như ssh.
 

và dùng mobaxterm để load file để tạo key đăng nhập.

<img src="http://i.imgur.com/VG4044w.png">

vào tool trong mobaxterm chọn 

<img src="http://i.imgur.com/rASGWIe.png">

chọn đường link đến file minh vừa copy từ máy ảo ra và nhập pass của file đó. Sau khi load xong ta lưu lại private key với đuôi .ppk

<img src="http://i.imgur.com/euAve5p.png">

Mở mobaxterm lên chọn các thông số như địa chỉ host và chọn đường dẫn file .ppk vừa tạo.

<img src="http://i.imgur.com/TTYAFO5.png">

Kết quả:

<img src="http://i.imgur.com/DDOMddb.png">

