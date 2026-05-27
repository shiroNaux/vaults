---
aliases:
  - dlt
tags:
  - databricks
  - dlt
---
> Hiện tại Databricks đã thay thế dlt bằng phiên bản open source [[Apache Spark|Spark]] Declarative Pipeline. Các code cũ chỉ cần thay thế annotation @dlt bằng @dp.
> Document chi tiết xem tại: [Spark Declarative Pipelines Programming Guide - Spark 4.1.2 Documentation](https://spark.apache.org/docs/latest/declarative-pipelines-programming-guide.html)
# Glossaries
- **Expectation**: Ý nghĩa cũng giống như expectations trong [[Great Expectations]], là các metrics define data quality
- **Event Log**: The DLT event log is a system-generated Delta table that records detailed metadata about every pipeline run, including expectation pass/fail counts, data quality metrics, table update durations, and cluster utilization. Engineers query the event log programmatically or inspect it through the pipeline UI to monitor pipeline health, diagnose failures, track data quality trends, and set up automated alerts on quality regressions.

---
# Introduction

Delta live table là 1 công cụ dùng để thực hiện transform dữ liệu. Đây là 1 sản phẩm proprietary của [[Databricks]], không có phiên bản open source.
Các đặc trưng của delta live table
- Có thể được sử dụng bằng SQL hoặc Python -> kết hợp của [[dbt]] và [[Apache Spark]]
- Hỗ trợ cả streaming và batch process
- Sản phẩm priority của Databrick (closed source)


# Mode
[[Databricks]] có 3 cách để xử lý các row vi phạm expectation
- ***EXPECT***: Các rows vẫn sẽ được ghi vào table và pipeline vẫn sẽ được tiếp tục. Chỉ có log *warning* được emit ra cho user. Đây là mode default của Databricks
- ***EXPECT OR DROP***: Các rows vi phạm expectation sẽ bị xóa khỏi bảng đích, pipeline vẫn tiếp tục chạy
- ***EXPECT OR FAIL***: Mỗi khi có vi phạm expectation thì pipeline sẽ bị dừng lại ngay và mark là failed.


# Feature
1. dlt có thể được viết 
# Alternative
1. [[dbt]]
2. [[Apache Spark|Spark]] Declarative Pipelines