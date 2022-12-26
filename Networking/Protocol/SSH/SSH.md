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
# References

1. [Ssh-agent single sign-on configuration, agent forwarding, the agent protocol.](https://www.ssh.com/academy/ssh/agent)
2. [Manage SSH-keys with the SSH-agent - Experiencing Technology (tinned-software.net)](https://blog.tinned-software.net/manage-ssh-keys-with-the-ssh-agent/)
3. [What is the difference between ssh-add and ssh-agent? - Stack Overflow](https://stackoverflow.com/questions/22272299/what-is-the-difference-between-ssh-add-and-ssh-agent)