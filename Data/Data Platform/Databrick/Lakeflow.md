---
aliases:
---
# Introduction
Lakeflow là bản nâng cấp của [[Delta live table|dlt]] dựa trên [[Apache Spark]] Declerative Pipeline
So với dlt - mục đích chủ yếu là data tranformation, Lakeflow có thêm nhiều các tính năng về ingestion data. Cho phép người dùng config các source và sink connector đến các nguồn cơ bản như: [[Database]],...

Thêm vào đó, Lakeflow cũng cho phép đọc và ghi dữ liệu streaming từ các nguồn như: [[Apache Kafka|kafka]],...

# Zerobus ingest
It is a **push-based ingestion API** designed for streaming data like _IoT_, _telemetry_, and _event data_

Tức là [[Databricks]] sẽ cung cấp 1 Zerobus server(là 1 thành phần của Lakeflow) để nhận data. Để có thể push data qua zerobus, người dùng sẽ phải sử dụng [[Python]] hoặc 1 số ngôn ngữ được hỗ trợ khác, cài zerobus sdk để sử dụng cho việc push data