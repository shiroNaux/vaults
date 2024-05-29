---
tags:
---
# Introduction

LDAP là viết tắt của Lightweight Directory Access Protocol -> Nó là 1 Giao thức (Protocol) dùng để lấy thông tin đưuọc lưu trữ dưới dạng các directory thông qua network.

Bản chất thì LDAP chỉ là giao thức, cho nên nó không lưu trữ thông tin. Thông tin sẽ được lưu trữ trên Directory server hoặc còn được gọi là LDAP server.

LDAP thường được dùng kết hợp với [[Single Sign On|SSO]] để xác thực user tập trung. LDAP server sẽ đóng vai trò như 1 [[Identity Provider|IDP]]. Còn LDAP sẽ là giao thức mà ứng dụng SSO dùng để lấy đưuọc thông tin từ LDAP server.

# LDAP Server

Có 1 số giải pháo LDAP server phổ biến là:
- Active Directory: Hỗ trợ cả [[Kerberos]] và LDAP

# Features

- LDAP là giao thức dùng để lấy thông tin.
- Dùng chủ yếu trong việc [[Authentication]]
- 
## Pros
## Cons
- LDAP được optimize cho việc sử dụng 1 domain duy nhất. Do đó nếu muốn sử dụng LDAP cho nhiều domain khác nhau thì nên cẩn trọng
