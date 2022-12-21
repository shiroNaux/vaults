---
---

# Introduction

- CSRF là viết tắt của: Cross-Site Request Forgery, là 1 kĩ thuật tấn công mạng

CSRF là kỹ thuật tấn công dựa vào các thông tin được lưu trên trình duyệt của người dùng. Khi người dùng truy cập và trang web độc hại, chúng có thể gọi request tới trang web khác nhờ các thông tin đã được lưu trên trình duyệt. Do request được gửi bởi trình duyệt của người dùng, cho nên rất khó để có thể xác định được origin của request. Kỹ thuật này được miêu tả cụ thể theo các bước như sau:
- Người dùng truy cập vào 1 trang web và đã đăng nhập. Giả sử trang web là: `mybank.com`. Trang web này cho phép chuyển tiền thông qua 1 request có định dạng: `http://www.mybank.com/transfer?to=<SomeAccountnumber>;amount=<SomeAmount>`
- Người dùng truy cập vào 1 trang web khác, ví dụ: `www.cute-cat-pictures.org` là 1 trang web có chứa mã độc
- 

# Cách ngăn chặn

1. Sử dụng CSRF token

a

# References