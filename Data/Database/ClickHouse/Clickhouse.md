---
---

# Abstract

- ClickHouse là 1 [[database]] được viết bằng [[C++]], bởi Yandex
- ClickHouse được phát triển để đáp ứng các nhu cầu về [[OLAP]] cũng như các nhu cầu về xử lý realtime
- Là 1 database hướng cột

# Architecture

## 1. Data store

Do ClickHouse là columnar database, nên khi lưu dữ liệu vào disk, có 2 cách tổ chức dữ liệu:

- `wide format` -> Mỗi column trong bảng được lưu ra 1 file riêng biệt
- `compact format` -> tất cả các cột đều lưu trong 1 `data file` -> `compact format` sẽ phù hợp hơn với các dữ liệu thay đổi (__UPDATE__) thường xuyên

Đơn vị lưu trũ nhỏ nhất trong ClickHouse là ___granule___, mỗi ___granule___ sẽ gồm n rows data được xác định từ lúc tạo bảng, giá trị mặc định là 8192.
## 2. Index
	
### Primary Index

ClickHouse sử dụng sparse index cho primary index. Đon giản là: ClickHouse sẽ không lưu trữ index theo từng dòng giống như các [[Relational Database|RDBMS]] khác, mà xẽ đánh index dựa vào các giá trị lớn nhất, nhỏ nhất trong ___granule___. Sparse index khá giống với partitiontrong 1 số [[Relational Database|RDBMS]], cũng như là partition của chính ClickHouse

Thông thường, với primary key thì các giá trị phải là not null, tuy nhiên do ClickHouse không có những yêu cầu chặt chẽ như các DBMS khác, nên các cột dùng làm `primary key` hay `order by` có thể chứa null. Cái này cần phải enable setting và lựa chọn các giá trị null sẽ được lưu ở các dòng đầu hay cuối bảng.

#### Cách hoạt động của primary key
Để hình dung cách hoạt động của primary key chúng ta sẽ sử dụng ví dụ sau
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

#### Primary Index file

Primary index được lưu trữ trong file có tên là `primary.idx`. File này là 1 uncompressed flat array file, tức là 1 file ko nén chứa các entry liên tiếp nhau giống như 1 array. Minh họa của file `primary.idx`

![[sparse-primary-indexes-03a-d89d998e12380419456e8b198110752b.png]]
- File `primary.idx` có số entry bằng với số granules của bảng, trong ví dụ này là 1083 entries hay mark (array bắt đầu = 0)
- Mỗi 1 entry sẽ bao gồm n giá trị (với n là số cột trong primary key, trong ví dụ này sẽ là 2), tương ứng với granule mà entry này đại diện:
	- Giá trị thứ nhất là: ___giá trị nhỏ nhất___ trong granule tương ứng của cột ___đầu tiên___ trong __primary key__
	- Giá trị thứ hai là:   ___giá trị nhỏ nhất___ trong granule tương ứng của cột ___thứ 2___ trong __primary key__
- Chú ý: Các giá trị trong cùng 1 entry không có quan hệ gì với nhau hết, nó chỉ là các giá trị nhỏ nhất trong granule tương ứng
- Toàn bộ file `primary.idx` sẽ được load lên RAM trong quá trình xử lý -> Nếu primary key quá nhiều cột có thể dẫn tới __OOM__

![[sparse-primary-indexes-03b-a5f733bcca895013c09f3cb54a1b3681.png]]
### Index in query execution

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
	- block offset: là vị trí của granule lưu trong `data file`. Data file là file đã compressed. Giá trị offset này có thể không chỉ chứa 1 mà có nhiều granule khác. 
	- Sau khi xác định được vị trí của data, chúng được đưa lên memory và uncompressed. Giá trị thứ 2 trong `mark file`: granule offset chính là  vị trí của granule cần lấy trong data sau khi đã uncompressed

### Skipping index

Skipping index hay còn được gọi là secondary index, là các index khác primary index, được thêm vào để hỗ trợ cho các [[SQL|query]] trên các cột không nằm trong `Order by`.

Mô tả trực quan cho skipping index

![[simple_skip-52c0988d126255fdb7aa8ad94036b1f7.svg]]

Nó cũng khác giống với secondary index trong các [[Relational Database|RDBMS]]. Tuy nhiên do ClickHouse lưu dữ liệu dưới dạng cột, cũng như đơn vị lưu trữ là granule cho nên skipping index sẽ không sử dụng B-Tree hay các cấu trúc index tương tự. Skipping index của ClickHouse cũng có cấu trúc giống như `idx file` hay `mark file`, lưu trữ theo granule -> mỗi entry tương ứng với 1 granule. Các giá trị tương ứng với các granule này tùy thuộc vào type của index. Có 3 type chính:
- Min - Max của column
- Set -> tập hợp tất cả các giá trị unique
- Bloom filter

Các giá trị này được sử dụng để loại bỏ (skipping) bớt các granule khi nó được nhắc đến trong [[SQL|query]]. Do đó có thể thấy là:
- Skipping index performance bị ảnh hưởng bởi `Order by` -> Nếu `Order by` quá chi tiết (high cardinity) thì skipping index sẽ không được hưởng lợi quá nhiều, do có distribution của column được đánh index bị áp chế bởi các cột trong `Order by`
- Skipping index hoạt động giống như các cột phụ của `Order by`, thích hợp để sử dụng trong 1 số trường hợp dùng các preaggregate table engine, khi mà các cột Order by có nhiệm vụ đặc biệt.

### Primary key vs Order by

Đối với các MergeTree [[ClickHouse Table Engine|table engine]], có 2 options là `Order by` và `Primary key` dều dùng để chỉ định các cột dùng để sắp xếp dữ liệu trên [[disk]]. Tuy nhiên Primary key phải là prefix của `Order by` bởi vì:
- Dữ liệu sẽ được sắp xếp theo `Order by`
- Tuy nhiên `Primary key` dùng để tạo `primary index`. `Primary index` sẽ được tạo ra dựa trên các cột dùng để sắp xếp dữ liệu -> `Primary key` phải là prefix của `Order by`

## 3. Partition

Parttion trong ClickHouse tương đối giống với partition trong các [[Relational Database|RDBMS]] khác. Đều là chia bảng ra thành các bảng con theo chiều dọc để xử lý các câu truy vấn theo điều kiện nhanh hơn. Tuy nhiên vẫn có sự khác biệt giữa ClickHouse với các [[Relational Database|RDBMS]] khác, cụ thể ở đây là [[PostgreSQL]]

- ClickHouse chỉ có partition theo 1 level, trong khi [[PostgreSQL]] hỗ trợ multi level partitions

## 4. Projection

Projection là 1 cách thức mới, nhằm tăng độ hiệu quả cho các câu truy vấn mà:
- Điều kiện `where` không chứa các column nằm trong `primary key`
- Thực hiện Pre-aggregate các columns -> giảm tài thời gian tính toán cũng như I/O

Projection thực chất là lưu thêm 1 hay nhiều các bảng ẩn kèm theo bảng chính. Các bảng này có dữ liệu được phân phối từ bảng chính, tuy nhiên sẽ được sắp xếp hoặc aggreagte theo 1 cách khác so với bảng chính

Ví dụ:

```SQL
CREATE TABLE visits  
(  
`user_id` UInt64,  
`user_name` String,  
`pages_visited` Nullable(Float64),  
`user_agent` String,  
PROJECTION projection_visits_by_user  
(  
SELECT  
user_agent,  
sum(pages_visited)  
GROUP BY user_id, user_agent  
)  
)  
ENGINE = MergeTree()  
ORDER BY user_agent
```
 Như trong ví dụ trên, ngoài bảng chính là `visits`, thì còn 1 bảng phụ nữa `projection_visits_by_user` chứa các thông tin giống với bảng chính, nhưng đã được aggregate theo `user_id` và `user_agent`. Mỗi khi có 1 câu [[SQL|query]] mà có nội dung là tính tổng `pages_visited` theo `user_id` hay `user_agent` thì ClickHouse sẽ không đọc dữ liệu tù bảng mà lấy dữ liệu từ projection đã được aggregate và sắp sếp theo `user_id` -> tăng tốc độ truy vấn

### Projection vs Materialized view

Projection thường được so sánh với materialized view bởi vì đây là 2 cách phổ biến để tăng tốc truy vấn bằng cách tạo thêm bảng hay các thành phần lưu trữ vật lý khác(tức là tăng không gian để đổi lấy thời gian).

Về cơ bản thì Projection và Materialized view đều lấy dữ liệu sau khi chúng được insert thành công vào bảng chính và từ đó xử lý để tạo ra các bảng phụ. Cách hoạt động rất giống với trigger trong các [[Relational Database]].

Tuy nhiên so với Materialized view, Projection có các điểm sau

##### Advantages
- Nếu view thì sẽ phải sửa câu query, còn nếu dùng projection thì sẽ không cần phải sửa query (tên bảng), ClickHouse sẽ tự tìm projection hù hợp theo câu truy vấn
- Dữ liệu của materialized view được sinh ra sau khi nó được insert thành công vào bảng chính và thực hiện thêm các bước xử lý được define trong view. Quá trình này có thể xảy ra lỗi, tức là khi dữ liệu đã được insert vào bảng nhưng trong view lại không có -> __miss data__. Còn dữ liệu của projection được sinh ra dưới background. Nếu xảy ra lỗi, nó có thể mix data từ projection và bảng chính để đưa ra kết quả, hoặc sẽ raise 1 exception nếu người dùng truy vấn -> Khả năng __handle lỗi__ của projection tốt hơn
##### Disadvantages
- Projection không làm thay đổi data quá nhiều, hầu hết nó sẽ được dùng để có nhiều cách  sắp xếp dữ liệu để câu truy vấn có hiệu năng tốt hơn. Còn materialized view thì có khả năng xử lý dữ liệu ở mức cao hơn, do nó có thể kết hợp với các bảng khác hay sử dụng nhiều hàm khác. -> Materialized view xử lý dữ liệu tốt hơn

# References
1. [What Is ClickHouse? | ClickHouse Docs](https://clickhouse.com/docs/en/intro/)
2. [ClickHouse Index Design | ClickHouse Docs](https://clickhouse.com/docs/en/guides/improving-query-performance/sparse-primary-indexes/sparse-primary-indexes-design/)
3. [Experimental ClickHouse: Projections・Tinybird](https://www.tinybird.co/blog-posts/projections)