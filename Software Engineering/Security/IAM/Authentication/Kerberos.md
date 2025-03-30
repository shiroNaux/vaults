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
2. Yêu cầu truy cập dịch vụ.
	- Mỗi khi client muốn truy cập vào 1 dịch vụ, client sẽ phải gửi Ticket Granting Service (TGS-REQ) đến KDC, bao gồm TGT và tên dịch vụ cần truy cập. 
	- KDC sẽ giải mã TGT để kiểm tra. Nếu OK sẽ gửi về cho client Service ticket dưới dạng TGS (Ticket Granting Service Response - TGS-REP). Service Ticket được mã hóa bằng khóa bí mật chung giữa KDC và Service.
3. Client xác thực với Service.
	- Client gửi yêu cầu dịch vụ (Service Request - S-REQ) đến Service, kèm theo Service Ticket.
	- Service giải mã Service Ticket, kiểm tra tính hợp lệ của yêu cầu và thông tin Client. Nếu đúng, Service sẽ xác nhận xác thực thành công và cung cấp dịch vụ cho Client.

## KDC

KDC - Key distribution center

### Authentication Server (AS)
### Ticker-Granting Sever (TGS)
### Database
Usually is a local file in the AS

# Purposes
1. Secure authentication
2. SSO
3. Network security: -> Prevent transmit password over network -> better security
4. Service authentication