---
aliases:
  - dlt
tags:
  - databricks
  - dlt
---
# Glossaries
- **Expectation**: Ý nghĩa cũng giống như expectations trong [[Great Expectations]], là các metrics define data quality

---
# Introduction

Delta live table là 1 công cụ dùng để thực hiện transform dữ liệu. Đây là 1 sản phẩm priority của [[Databricks]], không có phiên bản open source.
Các đặc trưng của delta live table
- Có thể được sử dụng bằng SQL hoặc Python -> kết hợp của [[dbt]] và [[Apache Spark]]
- Hỗ trợ cả streaming và batch process
- Sản phẩm priority của Databrick (closed source)


# Mode
[[Databricks]] có 3 cách để xử lý các row vi phạm expectation
- ***EXPECT***: Các rows vẫn sẽ được ghi vào table và pipeline vẫn sẽ được tiếp tục. Chỉ có log *warning* được emit ra cho user. Đây là mode default của Databricks
- ***EXPECT OR DROP***: Các rows vi phạm expectation sẽ bị xóa khỏi bảng đích, pipeline vẫn tiếp tục chạy
- ***EXPECT OR FAIL***: Mỗi khi có vi phạm expectation thì pipeline sẽ bị dừng lại ngay và mark là failed.


# Alternative
1. [[dbt]]
2. [[Apache Spark]]
- bản thân của Delta live table được phát triển dựa trên Apache Spark, thực chất chỉ là parse từ những câu lện [[SQL]] hay [[python]] ra Spark code rồi thực thi