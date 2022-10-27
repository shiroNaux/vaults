---
---
# Single Sign On
### 1. Introduction
- Single sing on là cơ chế để đăng nhập nhiều dịch vụ khác nhau thông qua 1 service duy nhất.
- SSO chủ yếu thực hiện tác vụ authentication chứ không bao gồm authorization trong nó

### SSO flow
![[../../../_images/typical-sso-v2.png]]

Các bước thực hiện quá trình SSO
1. Người dùng gửi request đến dịch vụ (Service Provider / SP)
2. SP kiểm tra user đã đăng nhập hay chưa (thông qua token, cookies, ...), nếu chưa thì sẽ redirect user đó đến trang web của Identity Provider để đăng nhập
3. Người dùng sẽ thực hiện đăng nhập qua Identity Provider, hoặc là sử dụng cookies, token của session đã được
4. 

## 2. Implement SSO
Một số protocol thường dùng để implement SSO là
- [[OpenID Connect]]
- [[LDAP]]
- [[SAML]]