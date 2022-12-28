---
---
# Introduction

ClickHouse có 1 khái niệm gọi là Table Engine. Table Engine chính là cách mà ClickHouse lưu trữ và xử lý dữ liệu ở mức vật lý. Khi tạo bảng, ta cần phải chỉ ra nó sẽ sử dụng Table Engine nào -> chỉ ra cách lưu trữ vật lý của bảng.

Các Table Engine sẽ được đặc trưng bởi các yếu tố:
- Cách thức, vị trí lưu trữ dữ liệu và cách đọc, ghi dữ liệu
- Những câu [[SQL|truy vấn]] nào có thể được sử dụng
- Concurrent data access.
- Indexes.
- Multithread request execution.
- Data replication.

# Engine Family

## MergeTree

Đây có lẽ là Engine được biết đến và sử dụng nhiều nhất trong ClickHouse. MergeTree engine family bao gồm:
- MergeTree
- ReplacingMergeTree
- SummingMergeTree
- AggregatingMergeTree
- CollapsingMergeTree
- VersionedCollapsingMergeTree
- GraphiteMergeTree

### 

## Log

## Integration Engines

## Special Engines

## Virtual Column