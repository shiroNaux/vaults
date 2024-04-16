---
---

# Abstract

OAuth2 (viết tắt của Open Authorization), là 1 tiêu chuẩn (hoặc còn được gọi là framework) dùng để phân quyền.

# Specification

## Glossaries

- Resource Ownner: Owner của resource -> người có thể grant access vào resource cho các dối tượng khác
- Resource Server: Nơi chứa resource
- Authorization Server: 
- Access Token
- Authorization Grant
- Client
- Confidential Client
- Public Clients
- Authorization Code
- Scope
- Refresh Token
- Redirect URI

# Request Flow


Để lấy được Access Token, OAuth2 cung cấp 4 flow khác nhau, mỗi flow thì sẽ phù hợp với từng use case khác nhau.
Các flow đó bao gồm:
- Authorization Code Flow
- Implicit Flow with Form Post
- Resource Owner Password Flow
- Clent Credentials Flow

OAuth sử dụng 1 số API endpoints nhất định để client gọi request:
- `/authorie`: 
- `/oauth/token`:
# References
1. [An overview of OAuth2 concepts and use cases | Ory](https://www.ory.sh/docs/oauth2-oidc/overview/oauth2-concepts)
2. [OAuth vs. JWT: What's the Difference for Application Development | Permit](https://www.permit.io/blog/differences-between-oauth-vs-jwt)