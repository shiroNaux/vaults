---
---

# Introduction

- CSRF là viết tắt của: Cross-Site Request Forgery, là 1 kĩ thuật tấn công mạng

CSRF là kỹ thuật tấn công dựa vào các thông tin được lưu trên trình duyệt của người dùng. Khi người dùng truy cập và trang web độc hại, chúng có thể gọi request tới trang web khác nhờ các thông tin đã được lưu trên trình duyệt. Do request được gửi bởi trình duyệt của người dùng, cho nên rất khó để có thể xác định được origin của request. Kỹ thuật này được miêu tả cụ thể theo các bước như sau:
- Người dùng truy cập vào 1 trang web và đã đăng nhập. Giả sử trang web là: `mybank.com`. Trang web này cho phép chuyển tiền thông qua 1 request có định dạng: `http://www.mybank.com/transfer?to=<SomeAccountnumber>;amount=<SomeAmount>`
- Người dùng truy cập vào 1 trang web khác, ví dụ: `www.cute-cat-pictures.org` là 1 trang web có chứa mã độc
- Trang web này  có thể khiến trình duyệt gửi 1 request (gắn trong [[HTML]] hoặc bất cứ cách nào khác) với nội dung: `http://www.mybank.com/transfer?to=123456;amount=10000`.
- Khi đó request sẽ được gửi đến `mybank.com` và server sẽ không phát hiện được request đó là giả mạo bởi vì nó được gửi từ trình duyệt của người dùng.

=> Server sẽ thực hiện request đó -> 1 vụ tấn công vừa được thực hiện
# Cách ngăn chặn

1. Sử dụng CSRF token

Với mỗi page, form, ... mà trang web gửi đến người dùng sẽ luôn kèm theo 1 token gọi là CSRF Token. Nó là 1 số ngẫu nhiêu, rất lớn mà không thể đoán mà hay [[brute force]] được. Và mỗi request thật từ người dùng đến server sẽ luôn kèm theo token này để đối chứng xem request này có được thực hiện từ page mà server đã phản hồi lại cho người dùng hay không.

Và các trang web giả mạo khác cũng sẽ không biết được các giá trị này do đã bị phía browser chặn access.

# References
1. [What is a CSRF token? What is its importance and how does it work? - Stack Overflow](https://stackoverflow.com/questions/5207160/what-is-a-csrf-token-what-is-its-importance-and-how-does-it-work)
2. [CSRF tokens | Web Security Academy (portswigger.net)](https://portswigger.net/web-security/csrf/tokens)