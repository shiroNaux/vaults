---
---
# Introduction

ClickHouse có 1 khái niệm gọi là Table Engine. Table Engine chính là cách mà ClickHouse lưu trữ và xử lý dữ liệu ở mức vật lý. Khi tạo bảng, ta cần phải chỉ ra nó sẽ sử dụng Table Engine nào -> chỉ ra cách lưu trữ vật lý của bảng.

Các Table Engine sẽ được đặc trưng bởi các yếu tố:
- How and where data is stored, where to write it to, and where to read it from.
- Which queries are supported, and how
- Concurrent data access
- Use of indexes, if present
- Whether multithread request execution is possible
- 