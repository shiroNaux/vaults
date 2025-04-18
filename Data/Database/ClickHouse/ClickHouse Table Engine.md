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

Đây là Engine phổ biến nhất của [[ClickHouse]]. MergeTree engine family bao gồm các table engine:
- [MergeTree](#MergeTree table Engine)
- ReplacingMergeTree
- SummingMergeTree
- AggregatingMergeTree
- CollapsingMergeTree
- VersionedCollapsingMergeTree
- GraphiteMergeTree

### MergeTree table Engine

MergeTree Engine có các tính năng chính sau:
- Data được lưu trữ trên [[Hard disk|ổ cứng]] và được sắp xếp theo
- Có thể chia partition cho dữ liệu
- Hỗ trợ replication
- Hỗ trợ sampling

Các đặc điểm của MergeTree:
- 

### ReplacingMergeTree

### SummingMergeTree

### AggregatingMergeTree

### CollapsingMergeTree

### VersionedCollapsingMergeTree

### GraphiteMergeTree

## Log

Các Engine thuộc dạng Log thường được thiết kế đặc biệt cho các workload với đặc điểm: quick write vào nhiều bảng nhỏ, và read tất cả các bảng với nhau. Log Engine family bao gồm:
- TinyLog
- StripleLog
- Log

## Integration Engines

Các Engine thuộc loại Interagtion sẽ đọc data được lưu trữ từ 1 nơi khác không phải ClickHouse, lấy dữ liệu về và xử lý. Dữ liệu sẽ không được lưu trữ bởi ClickHouse. Nó tương tự như tính năng FDW của [[PostgreSQL]]. Các Engine thuộc loại Integration là:
- ODBC
- JDBC
- [[MySQL]]
- [[MongoDB]]
- [[HDFS]]
- [[S3]]
- [[Apache Kafka|Kafka]]
- EmbbededRocksDB
- [[PostgreSQL]]
## Special Engines

Các Engine đặc biệt:
- 

## Virtual Column

# References
1. [Table Engines | ClickHouse Docs](https://clickhouse.com/docs/en/engines/table-engines/)
2. [How to pick an ORDER BY / PRIMARY KEY / PARTITION BY for the MergeTree-family table | Altinity Knowledge Base](https://kb.altinity.com/engines/mergetree-table-engine-family/pick-keys/)
3. [Understanding ClickHouse Data Skipping Indexes | ClickHouse Docs](https://clickhouse.com/docs/en/guides/improving-query-performance/skipping-indexes/)