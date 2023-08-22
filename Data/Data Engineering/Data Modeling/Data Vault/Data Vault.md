---
---
# Abstract

Data vault là 1 pháp data modeling. Nó được phát triển sau [[Dimensional Modeling]] nên nó hạn chế được 1 số nhược điểm của Dimensional Modeling.

# Architecture

Ở đây, chúng ta sẽ nói đến Data Vault 2.0, là phiên bản được sử dụng ở thời điểm hiện tại (([[2023-08-23]]))

- Data vault modeling là 1 thành phần của Data Vault

Các bảng trong Data Vault Modeling được chia ra làm 3 loại chính
- Hub -> Các bussiness __entities__ được quan tâm
- Link -> Liên kết/Link giữa các Hub với nhau.
- Satellites -> Là bảng chứa các thông tin chi tiết, bổ sung cho Hub và Link

## Structure
### Hubs
### Links

#### Hierarchical Link
#### Same-as Link

#### Non-historized Link

#### Exploration Link

### Satellites

#### Multi-active satellite
#### Efective satellite

#### System-driven satellite

### Additional Table

#### Point-in-time table (PIT)
#### Bridge table
#### Reference table

#### Staging table
# Modeling steps

Có nhiều cách tiếp cận với 
# Comparation

## Pros
- Agile (???) -> need prove
- Structured, dễ dàng thay đổi (flexibility for refactoring)
- Có các khái niệm tương đồng với các mô hình khác -> dễ làm quen và nắm bắt cách dùng
## Cons
# References
1. [How to implement data vault model - Aginic](https://aginic.com/blog/modelling-with-data-vaults/)