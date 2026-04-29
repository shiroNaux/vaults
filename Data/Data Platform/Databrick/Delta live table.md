---
aliases:
  - dlt
tags:
  - databricks
  - dlt
---
# Introduction

Delta live table là 1 công cụ dùng để thực hiện transform dữ liệu. Đây là 1 sản phẩm priority của [[Databricks]], không có phiên bản open source.
Các đặc trưng của delta live table
- Có thể được sử dụng bằng SQL hoặc Python -> kết hợp của [[dbt]] và [[Apache Spark]]
- Hỗ trợ cả streaming và batch process
- Sản phẩm priority của Databrick (closed source)

# Alternative
1. [[dbt]]
2. [[Apache Spark]]
- bản thân của Delta live table được phát triển dựa trên Apache Spark, thực chất chỉ là parse từ những câu lện [[SQL]] hay [[python]] ra Spark code rồi thực thi