---
tags:
---
# Abstraction

SAML - Security Assertion Markup Language
SAML is an open standard for exchanging authentication and authorization data between parties, typically between an **Identity Provider (IdP)** and a **Service Provider (SP)**.

# SAML Flow

1. User request to SP -> Service provider redirect user to IdP
2. User enter credential to login to IdP
3. IdP sẽ tạo Assertion (XML format) và gửi nó về cho SP (thông qua redirect)
4. SP sẽ nhận verify dựa trên dữ liệu từ Assertion

Luồng hoạt động của SAML khá giống với OIDC, khác nhau chủ yếu ở các điểm:
- [[protocol|Giao thức]]
- Token format: SAML sử dụng XML, còn [[OIDC]] sử dụng [[JWT]]

## SAML Authentication request
## SAML Response 



# Pros
# Cons

# Comparison


# References
1. [SAML vs. LDAP: How to Choose The Right Protocol — WorkOS](https://workos.com/blog/saml-vs-ldap-how-to-choose-the-right-protocol)
2. 