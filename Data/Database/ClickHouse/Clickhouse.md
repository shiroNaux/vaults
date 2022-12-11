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

ClickHouse sử dụng sparse index cho primary index. Đon giản là: ClickHouse sẽ không lưu trữ index the từng dòng giống như các [[Relational Database|RDBMS]] khác, mà sẽ lưu trữ index theo các __granule__. Sparse index khá giống với partitiontrong 1 số [[Relational Database|RDBMS]]

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
- Mỗi bảng chỉ có duy nhất 1 primary key, do nó xác định thứ tự lưu trữ vật lý.

Index được lưu trên disk theo ascending order.

![[sparse-primary-indexes-01-2941706e5275aed3ef7e0ea242196c41.png]]

Như đã đề cập ở trên, ClickHouse sẽ sử dụng sparse index, tức là sẽ gôm nhiều rows vào thành 1 granule.

![[sparse-primary-indexes-02-2b2fa8c54c018fd39db9c8a2939dc12c.png]]
Việc lưu trữ này sẽ đem lại 1 số lợi ích so với index thông thường
- Thay vì scan toàn bộ 8.87 triệu dòng, thì nếu sùng sparse index chỉ cần scan 1082 granules để tìm ra được granule thích hợp rồi tiếp tục xử lý -> chạy nhanh hơn trong các trường hợp sử dụng __where__ với primary key

### Index file

Primary index được lưu trữ trong file có tên là `primary.idx`. File này là 1 uncompressed flat array file, tức là 1 file ko nén chứa các entry liên tiếp nhau giống như 1 array. Minh họa của file `primary.idx`

![[sparse-primary-indexes-03a-d89d998e12380419456e8b198110752b.png]]
- File `primary.idx` có số entry bằng với số granules của bảng, trong ví dụ này là 1083 entries hay mark (array bắt đầu = 0)
- Mỗi 1 entry sẽ bao gồm n giá trị (với n là số cột trong primary key, trong ví dụ này sẽ là 2), tương ứng với granule mà entry này đại diện:
	- Giá trị thứ nhất là: ___giá trị nhỏ nhất___ trong granule tương ứng của cột ___đầu tiên___ trong __primary key__
	- Giá trị thứ hai là:   ___giá trị nhỏ nhất___ trong granule tương ứng của cột ___thứ 2___ trong __primary key__
- Chú ý: Các giá trị trong cùng 1 entry không có quan hệ gì với nhau hết, nó chỉ là các giá trị nhỏ nhất trong granule tương ứng
- Toàn bộ file `primary.idx` sẽ được load lên RAM trong quá trình xử lý -> Nếu primary key quá nhiều cột có thể dẫn tới __OOM__

![[sparse-primary-indexes-03b-a5f733bcca895013c09f3cb54a1b3681.png]]
## 3. Index in query execution

Khi ClickHouse thực hiện 1 query, có thể chia ra làm 2 bước:
- First stage - granule selection: Lựa chọn ra những granule chứa data cần thiết
- Second stage - data reading: Từ những granule đã được lựa chọn, thực hiện load data từ disk lên [[RAM]]

### Locating granule

Sau khi đã tìm được granule có chứa data cần thiết. Việc tiếp theo là load data lên [[RAM]] để xử lý. Việc này được thực hiện nhờ vào các `mark file`.

Cấu trúc của 1 `mark file` như sau

![[sparse-primary-indexes-05-02e2356da8eb292a54513b2f587cfaf6.png]]
- Mỗi cột đều có 1 marfile riêng biệt, do ClickHouse lưu dữ liệu dưới dạng cột
- Cũng giống như `index file`, `mark file` cũng là __flat uncompressed array file__, mỗi một entry trong file cũng tương ứng với 1 granule của bảng
- Mỗi entry trong `mark file` có 2 giá trị:
	- block offset: là vị trí của granule lưu trong `data file`. Data file là file đã compressed. Giá trị offset này có thể không chỉ chứa 

# References
1. [What Is ClickHouse? | ClickHouse Docs](https://clickhouse.com/docs/en/intro/)
2. [ClickHouse Index Design | ClickHouse Docs](https://clickhouse.com/docs/en/guides/improving-query-performance/sparse-primary-indexes/sparse-primary-indexes-design/)
