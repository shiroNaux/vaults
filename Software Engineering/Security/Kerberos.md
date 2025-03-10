---
tags:
---
# Abstract
Kerberos là 1 giao thức authentication trong network, tức là các thành phần được xác thực qua mạng 

# Architecture

### Luồng hoạt động của Kerberos
1. Client gửi yêu cầu xác thực đến KDC.
	- Client gửi yêu cầu xác thực (Authentication Service Request - AS-REQ) đến KDC, bao gồm tên người dùng và tên dịch vụ mà người dùng muốn truy cập.
	- KDC kiểm tra thông tin, nếu hợp lệ sẽ trả về cho client TGT (Ticket granting ticket) dưới dạng Authentication Service Response (AS-REP)
	- ==**TGT được mã hóa bởi private key share chung giữa KDC và client**==
2. Yêu cầu truyb cập dịch vụ.
	- Mỗi khi client muốn truy cập vào 1 dịch vụ, client sẽ phải gửi yêu cầu đến KDC trước. Yêu cầu sẽ kèm theo Ticket đã được grant ở bước 1. 
3. Client xác thực với Service.

### Cấu trúc của KDC

KDC - Key distribution center