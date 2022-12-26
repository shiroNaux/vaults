Khi sử dụng [[SSH]], client hay server đều phải có key cho riêng mình. SSH Agent là 1 chương trình có nhiệm vụ quản lý các private key phía client, sử dụng chúng trong các kết nối [[SSH]]

# Functionalities

Thông thường, khi chúng ta sử dụng [[SSH]], thì ta cần phải chỉ rõ private key nào sẽ được sử dụng. Tuy nhiên có 2 cách để giảm thiểu thao tác này
- Sử dụng config file
- SSH Agent

SSH Agent có tác dụng quản lý các private key, mỗi khi 
Mặc định trên các [[Linux]] [[Operating System|OS]], thì SSH Agent đã được khởi động mỗi khi người dùng login -> không cần thêm thao tác nào nữa. Tuy nhiên, có thể khởi động 1 cách thủ công như sau:
``` bash
eval "ssh-agent"
```

Khi muốn thêm mới 1 private key, nếu chỉ đưa vào thư mục `~/.ssh`  là không đủ. Ta chỉ cần add private key đó và SSH Agent bằng lệnh
``` Bash
ssh-add ~/.ssh/id_rsa
```

# Detail process

## Negotiating Encryption for the Session

- Client sẽ tạo yêu cầu kết nối SSH đến server. Server sẽ tiếp nhận request và gửi về 1 response  bao gồm các thông tin
	- Version mà serever có thể support -> Nếu client không tương thích với các version này, connection sẽ kết thúc
	- Server public host key -> Client sẽ kiểm tra định danh của server dựa vào thông tin này
- Sau đó, client và server sẽ negotiate với nhau để tạo ra được 1 private key chung gọi là sahred session key, được lưu bởi cả client và server. Quá trình sử dụng thuật toán Diffie-Hellman, nó bao gồm các bước như sau:
	- 

## Authentication
- Sau khi đã thống nhất được key chung, tiếp đến là client sẽ đăng nhập vào server. Có 1 số cách authen chính:
	- password based: user sẽ nhập password
	- private key: Quá trình sử dụng private key sẽ trải qua các bước sau:
		- Client sẽ gửi ID của key mà nó muốn dùng để đăng nhập vào server
		- Server sẽ kiểm tra file `authorized_keys`, xem các public key trong đó có khớp với key mà client gửi đến hay không. 
		- Nếu tồn tại 1 key trùng, server sẽ tạo 1 số ngẫu nhiên và dùng public key vừa có để encrypt lại. Sau đó gửi giá trị vừa mã hóa cho client.
		- Client sẽ dùng private key để decrypt message và nhận được số mà server đã tạo nhẫu nhiên
		- Client kết hợp số vừa giải mã được và shared session key từ bước trước với nhau. Rối sau đó tính giá trị [[hash]] của giá trị trên. Sau đó gửi giá trị hash này đến server.
		- Server sẽ kiểm tra giá trị [[hash]] này (do cả client và server đều biết số nhẫu nhiên và shared session key) -> Nếu giá trị [[Hash]] trùng nhau thì server sẽ chấp nhận client là hợp lệ và bắt đầu thực hiện các thao tác chính của SSH
# References

1. [Ssh-agent single sign-on configuration, agent forwarding, the agent protocol.](https://www.ssh.com/academy/ssh/agent)
2. [Manage SSH-keys with the SSH-agent - Experiencing Technology (tinned-software.net)](https://blog.tinned-software.net/manage-ssh-keys-with-the-ssh-agent/)
3. [What is the difference between ssh-add and ssh-agent? - Stack Overflow](https://stackoverflow.com/questions/22272299/what-is-the-difference-between-ssh-add-and-ssh-agent)
4. [Best way to use multiple SSH private keys on one client - Stack Overflow](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)
5. [RFC 4253: The Secure Shell (SSH) Transport Layer Protocol (rfc-editor.org)](https://www.rfc-editor.org/rfc/rfc4253)
6. [authentication - How does SSH client ensure that SSH server bears the private key, which is the pair of the public key in client's "known_hosts" file? - Information Security Stack Exchange](https://security.stackexchange.com/questions/154796/how-does-ssh-client-ensure-that-ssh-server-bears-the-private-key-which-is-the-p)