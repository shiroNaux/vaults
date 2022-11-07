---
---

# Introduction

Delta live table là 1 công cụ dùng để thực hiện transform dữ liệu. Nó có những đặc điểm sau
- Sử dụng dưới dạng code SQL hoặc Python -> kết hợp của [[dbt]] và [[Apache Spark]]
- Hỗ trợ cả streaming và batch process
- Chỉ hoạt động trên nền tảng của Databrick

# Alternative
1. [[dbt]]
- 
3. [[Apache Spark]]
- bản thân của Delta live table được phát triển dựa trên Apache Spark, thực chất chỉ là parse từ những câu lện [[SQL]] hay [[python]] ra Spark code rồi thực thi