---
aliases:
---
# Introduction

Auto loader là công cụ dùng để ingest dữ liệu từ file vào các bảng trên [[Databricks]]. Auto loader có thể được chạy theo cả 2 mode: Streaming và Batch, bởi vì nó sử dụng [[Apache Spark|Spark]] để thực hiện việc đọc và ghi dữ liệu, nên nó tận dụng được cả 2 cơ chế này của Spark.

## File Detection mode
- **Directory Listing (Default):** Uses API calls to detect new files, managed via _RocksDB_ within the checkpoint.
- **File Notification:** Sets up notification and queuing services in your cloud account for event-based file detection.