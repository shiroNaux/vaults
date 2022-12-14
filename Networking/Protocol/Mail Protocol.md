---
---

### SMTP

smtp là viết tắt của _simple mail transfer protocol_, là 1 giao thức truyền thư điện tử qua mạng Internet. Tuy nhiên smtp chỉ là chuẩn outgoing hay sending email, tức là gửi thư đi mail server (phía server là ingoing). Còn đối với các ingoing mail (hay còn gọi là Retrieving emails, phía server sẽ trả lại email) các chuẩn POP3 và IMAP sẽ được sử dụng

smtp trước kia được dựa trên nền của [[FTP]], sau này được tách riêng ra
smtp được xây dựng phía trên của [[TCP]], và sử dụng cổng 25 là mặc định. smtp được sử dụng cùng với [[SSL]] được gọi là smtps và sử dụng cổng 465 làm cổng chuẩn.

### POP3
### IMAP

## Libraries

### Python
[[Python]] core cung cấp 1 số package để thực hiện các thao tác với email