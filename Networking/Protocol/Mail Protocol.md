---
---

# Mail Protocols

Hầu hết các giao thức gửi/nhận mail được dựa trên [[TCP]], và nằm ở tầng ___???___ của mô hình [[OSI]].

![[diagram-how-smtp-and-imap-pop3-work-together-pnap.png]]
Mô tả cách quá trình truyền email qua internet

### SMTP

smtp là viết tắt của _simple mail transfer protocol_, là 1 giao thức truyền thư điện tử qua mạng Internet. Tuy nhiên smtp chỉ là chuẩn outgoing hay sending email, tức là gửi thư đi mail server (phía server là ingoing). Còn đối với các ingoing mail (hay còn gọi là Retrieving emails, phía server sẽ trả lại email) các chuẩn POP3 và IMAP sẽ được sử dụng

smtp trước kia được dựa trên nền của [[FTP]], sau này được tách riêng ra
smtp được xây dựng phía trên của [[TCP]], và sử dụng cổng 25 là mặc định. smtp được sử dụng cùng với [[SSL]] được gọi là smtps và sử dụng cổng 465 làm cổng chuẩn.

### POP3



### IMAP

## Compare between protocols

|               | IMAP                                       | POP3                          | SMTP                                              |
| ------------- | ------------------------------------------ | ----------------------------- | ------------------------------------------------- |
| Function      | Retrieving emails                          | Retrieving emails             | Sending emails                                    |
| Port          | 143                                        | 110                           | 25                                                |
| Limitation    | Mailbox on the server has a definite quota | Messages will be deleted when | It has no ways of verifying sender -> Spam issues |
| Port with SSL | 993                                        | 995                           | 465                                               | 

Ngoài ra, giữa POP và IMAP cũng có 1 số điều cần lưu ý như sau:
| IMAP                                                                                               | POP                                                                                           |
| -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| IMAP sẽ chỉ show header, rồi sau đó dựa vào action của người dùng rồi mới download email về client | POP3 sẽ download tất cả các email đồng thời                                                   |
| IMAP vẫn lưu email trên server cho đến khi bị xóa -> server cần nhiều [[Hard disk]] hơn            | Email trên server sẽ bị xóa khi người dùng download về client -> server cần ít tài nguyên hơn |
|                                                                                                    |                                                                                               |

## Libraries
### Python
[[Python]] core cung cấp 1 số package để thực hiện các thao tác với email
- imaplib -> sử dụng giao thức IMAP
- [email]([email: Examples — Python 3.11.1 documentation](https://docs.python.org/3/library/email.examples.html))  -> thư viện để xử lý email, thường được dùng kết hợp với các thư viện khác
- smtplib -> dùng cho SMTP Protocol

# References

1. [IMAP vs. POP3 vs. SMTP: What Are the Differences? (phoenixnap.com)](https://phoenixnap.com/kb/imap-vs-pop3-vs-smtp)
2. [Difference Between SMTP, IMAP, And POP3 (Includes Comparisons) (salesblink.io)](https://salesblink.io/blog/difference-between-smtp-imap-pop3)
3. [SMTP vs IMAP vs POP3 - Knowing The Difference | JSCAPE](https://www.jscape.com/blog/smtp-vs-imap-vs-pop3-difference)