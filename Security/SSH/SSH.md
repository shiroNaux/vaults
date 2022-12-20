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
