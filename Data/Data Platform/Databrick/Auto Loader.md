---
aliases:
---
# Introduction

Auto loader là công cụ dùng để ingest dữ liệu từ file vào các bảng trên [[Databricks]]. Auto loader có thể được chạy theo cả 2 mode: Streaming và Batch, bởi vì nó sử dụng [[Apache Spark|Spark]] để thực hiện việc đọc và ghi dữ liệu, nên nó tận dụng được cả 2 cơ chế này của Spark.

## File Detection mode
- **Directory Listing (Default):** Uses API calls to detect new files, managed via _RocksDB_ within the checkpoint.
- **File Notification:** Sets up notification and queuing services in your cloud account for event-based file detection.

## Schema Evaluation Mode
Auto Loader offers four modes to handle schema changes when new files arrive:
- **`addNewColumns` (Default):** Automatically updates the schema at the defined location when new columns are detected (12:51-15:49).
- **`rescue`:** Pushes unexpected columns into a dedicated `_rescued_data` column instead of failing (15:49-18:29).
- **`none`:** Ignores any schema changes (18:29-20:45).
- **`failOnNewColumns`:** Stops the stream if the schema deviates from the current version (20:45-21:30).