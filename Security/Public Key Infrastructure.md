---
alias:
	- PKI
---
# Introduction

Public Key Infrastructure (PKI) là 1 công nghệ dùng đảm bảo tính an toàn trong việc trao đổi thông tin trong network. Cách hoạt động rất đơn giản: Sẽ có 1 bên thứ 3 (CA) đứng ra xác thực và đảm bảo cho các bên tham gia.
PKI hoạt động dựa trên [[Public Key Cryptography]]. Hệ thống PKI có nhiệm vụ tạo, quản lý các certificate. Các certificate này dùng để xác thực 1 entity cụ thể trong network. Thông thường khi nhắc đến PKI tức là ám chỉ đến công nghệ đảm bảo sự an toàn khi trao đổi thông tin bằng cách sử dụng [[Public Key Cryptography]]

Các thành phần của PKI bao gồm:
- Certificate authority -> có nhiệm vụ lưu trũ, xác thực các certificate
- Registration authority -> xác thực đối tượng gửi các yêu cầu đến hệ thống thông qua các certificate được lưu trữ bởi CA
- Central directory -> nơi lưu trữ các certificate -> có mức độ bảo mật rất cao.
- Certificate management system
- Certificate policy

# Digital certificate

Digital certificate đóng 1 vai trò quan trọng trong PKI. Nó có các chức năng sau:
- Nó chứa các thông tin và chủ sở hữu
- Có thời gian bắt đầu và hết hạn
- Được ký bởi các private certificate -> chống giả mạo
- Được phát hành bởi 1 tổ chức bên thứ 3 có uy tín
-> được coi là passport trong thế giới số

## Certificate Creation Process

Các bước trong quá trình tạo certificate
1. Privatekey được tạo dựa vào các yếu tố ngẫu nhiên. Và dựa vào private key đó, tiếp tục tạo public key.
2. Public key và các identifying attributes được encode thành Certificate Signing Request (CSR)
3. CSR sẽ được ký bởi private key của chủ sở hữu certificate -> chứng minh sự sở hữu đối với public key
4. Register sẽ gửi CSR đến RA (Registration authority) và signature đã được ký bởi private key, RA sẽ tiến hành xác minh. Nếu thành công, RA sẽ gửi thông tin về CA
5. CA sẽ xác minh và phát hành certificate cho register (Certificate sẽ được sign bởi private key của CA, do đó bất kỳ ai cũng có thể xác minh certificate bằng cách sử dụng public key của CA)
![[PKI-Certificate-Creation-Process.png]]

# Common Usage

Các công nghệ implement PKI thường thấy bao gồm

## 1. [[SSL Certificate]]

Việc áp dụng [[SSL]] lên các giao thức như [[HTTP]] khiến cho kết nối giữa các đối tượng trong network trở nên an toàn hơn đối với các cuộc tấn công
## 2. [[SSH]]

[[SSH]] cũng là 1 giao thức sử dụng PKI. Client sẽ dùng private key của mình để đăng nhập vào server, nơi mà đã có public key của client.

## 3. [[GPG]]
## 4. Email signing

# References 
1. [Public key infrastructure - Wikipedia](https://en.wikipedia.org/wiki/Public_key_infrastructure)
2. [What is PKI (Public Key Infrastructure)? (ssh.com)](https://www.ssh.com/academy/pki)
3. [Phần 1 : Tổng quan về PKI (viblo.asia)](https://viblo.asia/p/phan-1-tong-quan-ve-pki-1Je5EJo0KnL)
4. [What is PKI? A Public Key Infrastructure Definitive Guide – Keyfactor](https://www.keyfactor.com/resources/what-is-pki/)