---
---

# Abstract

- ClickHouse là 1 [[database]] được viết bằng [[C++]], bởi Yandex
- ClickHouse được phát triển để đáp ứng các nhu cầu về [[OLAP]] cũng như các nhu cầu về xử lý realtime
- Là 1 database hướng cột

# Architecture

## 1. Data store

## 2. Index

### Sparse Index

ClickHouse sử dụng sparse index cho primary index. Đon giản là: ClickHouse sẽ không lưu trữ index the từng dòng giống như các [[Relational Database|RDBMS]] khác, mà sẽ lưu trữ index theo các __granule__.

Để dễ hình dung chúng ta sẽ sử dụng ví dụ sau
 - Sử dụng bảng với nội dung như sau:
``` SQL
CREATE TABLE hits_UserID_URL  
	(  
	`UserID` UInt32,  
	`URL` String,  
	`EventTime` DateTime  
	)  
		ENGINE = MergeTree  
		PRIMARY KEY (UserID, URL)  
		ORDER BY (UserID, URL, EventTime)  
	SETTINGS index_granularity = 8192, index_granularity_bytes = 0;
```
- Dữ liệu trong bảng là khoảng 8.87 triệu dòng

Khi ta tạo bảng thuộc lớp ___MergeTree___, primary có thể được khai báo hoặc không
- Nếu không defined primary key -> primary key = `Order by`
- Nếu ta xác định primary key thì nó phải là prefix của `Order by`
- Khi ClickHouse thực hiện xử lý thì __primary index__ sẽ được load lên [[RAM]] dựa theo cá cột được defined trong _primary key_

Index được lưu trên disk theo ascending order.

![[sparse-primary-indexes-01-2941706e5275aed3ef7e0ea242196c41.png]]


# References
1. [What Is ClickHouse? | ClickHouse Docs](https://clickhouse.com/docs/en/intro/)
2. 
