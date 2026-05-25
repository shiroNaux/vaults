---
aliases:
---
# Dictionary
- **Control plane**:
- **Data plane**:
- **All-Purpose cluster**:
- **Job Cluster**:
- **SQL Warehouse**:
- **Service Principal**: Giống như role trong AWS, Service principal là credential dành cho các service, các automation  agent
- **Photon**:
- [[Unity Catalog|Catalog]]:
- [[Delta Lake]]:
- **Notebook**: 
- **Magic command**: Cũng giống như magic command trong Jupyter Notebook, được bắt đầu bằng ***`%`*** để thực hiện các câu lệnh đặc thù như cài package(%pip install)
- Transaction log
- **Managed table**: Là các table được quản lý (bao gồm cả data và metadata) bởi chính Databricks
- **External table**: Databricks sẽ có thông tin meta data của các table này, nhưng data file sẽ không được quản lý bởi Databricks
- **Schema enforcement**:
- **[[Delta live table]]**: 
- **Streaming table**: Là 1 loại table trong Databricks. Streaming table chỉ cho phép append thêm dữ liệu vào bảng và không được sửa đổi các record đã được ghi vào từ trước đó.
- **Materialized table**:
- **Auto loader**: Là 1 tính năng của Databricks cho phép auto load dữ liệu từ các nguồn Cloud storage. Databricks sẽ tự động nhận biết các file dữ liệu mới và ingest chúng về Databricks.
- **CloudFiles Format**: The cloudFiles format is the Spark data source identifier used to invoke Auto Loader when reading from cloud storage paths in Databricks. It is specified as the format parameter in spark.readStream.format and in DLT table definitions, and supports options for file type, schema hints, and schema evolution behavior.
- **Delta Sharing**: là 1 open protocol được Databricks phát triển với mục đích chia sẻ dữ liệu giữa các platform 1 cách hiệu quả, thống nhất.
- **DAB**:
- Landing zone: Là 1 phân vùng bên cạnh Raw - Silver - Gold layer trong [[Medallion Architecture]]. Đây là phân vùng lưu các file tạm, trước khi được ingest vào raw.
- Cluster pool
# Databricks Architecture

## Account
Trong Databricks, account là đơn vị cấu trúc cấp bậc cao nhất. Bên dưới account sẽ bao gồm các workspace là đơn vị logical để phân chia môi trường làm việc cho các workload khác nhau. Account còn dùng để quản lý users, group, service principal, SSO, ... nói chung là các vấn đề liên quan đến quyền truy cập dữ liệu và tính năng của Databricks. Account cũng thực hiện quản lý billing và Unity catalog metastore.

![[Pasted image 20260428184809.png]]

## Workspace
**Workspaces** are the collaboration environment where users run compute workloads such as ingestion, interactive exploration, scheduled jobs, and ML training.
## Unity Catalog metastores
**Unity Catalog metastores** are the central governance system for data assets such as tables and ML models. You organize data in a metastore under a three-level namespace
```
<catalog-name>.<schema-name>.<object-name>
```


## Role

Role được gán cho user trong 1 account để phân chia quyền hạn quản lý cho user đó.
Databricks có 4 roles:
- Account Administrator
- Metastore Administrator
- Workspace Administrator
- Owner


## Control plane

## Data plane

# [[Database]]
![[Pasted image 20260520000120.png]]

## Function
Có 2 loại function trong databricks
1. Built-in
2. UDF
Điều đặc biệt ở Databricks đó là UDF có thể được viết bằng cả [[Python|python]] và [[SQL]] (giống như viết function trong [[PostgreSQL]], tức là khai báo language trong lệnh create function). Và thậm chí code pyspark có thể call đến udf của sql thông qua expr
Ví dụ:
```
df = df.withColumn("a", expr("funtion_name(col_name)"))
```

## References
1. [High-level architecture | Databricks on AWS](https://docs.databricks.com/aws/en/getting-started/high-level-architecture)

# Cluster Structure

# Streaming

# Trigger mode

|                   | Triggered                      | Continous                    |
| ----------------- | ------------------------------ | ---------------------------- |
| Execution         | Run once then stop             | Keep running until stopped   |
| Data Process      | Process all available data     | New data as it arrives       |
| Cluster Lifecycle | Up during run -> cheaper       | Always on -> more expensive  |
| Latency           | 10 minutes, hourly, daily      | 10s -> a few minutes         |
| Trigger mechanism | Manual, scheduled, or via Jobs | Start once, always listening |

Databricks sử dụng [[Apache Spark]] làm key process engine, cho nên continous mode ở đây cũng hoạt động giống như streaming trong Apache Spark - theo micro batch.

Đối với continous mode, nếu ta không set trigger interval cho pipeline thì mỗi khi 1 microbatch chạy xong thì batch sau sẽ ngay lập tức chạy, điều này có thể kém hiệu quả trong 1 số trường hợp cụ thể. Do đó Databricks cho phép set trigger interval cho từng table hay function để người dùng có thể control được khoảng thời gian mà continous pipeline sẽ chạy.

> Trigger interval chỉ có thể được set cho continous pipeline

Vậy, điểm khác biệt lớn nhất giữa Triggered và Continous mode đó là về lượng data được xử lý. Triggered sẽ xử lý hết tất cả các data có thể, còn continous sẽ chỉ process các data new arrives. Và cả 2 đều có thể điều khiển được khoảng thời gian giữa 2 lần chạy.

# Catalog

## Standard
## Foregin
## Shared
## Lakebase Postgres

# Pool
[Connect to pools | Databricks on AWS](https://docs.databricks.com/aws/en/compute/pool-index)

# Git integraion
[Databricks Git folders | Databricks on AWS](https://docs.databricks.com/aws/en/repos/)


# Row level security

Để có thể sử dụng được Row level security trên Databricks, trước hết cần phải có 1 function mà output trả về là True hay False. Khi người dùng select dữ liệu từ bảng, function sẽ được evaluate cho từng row và sẽ chỉ return các row mà function trả về kết quả là True.

Để gán function cho bảng, sử dụng lệnh
```sql
Alter table <table_name> set row filter <function_name> on (column_name);
```

Funtion có thể nhận input đầu vào, đó chính là giá trị tương ứng với cột `column_name` trong lệnh alter table bên trên.
**-> Performance issues???**

# Advanced Security Features

## Column Level Masking

ew

## UNDROP

Databricks có tính năng giống như recycle bin, cho phép người dùng restore lại table trong vòng 7 ngày kể từ lúc thực hiện leennhj drop
-> Data được store ở đâu? có xóa data? có force delete mà ko cần undrop?

Databricks cũng có 1 lệnh [[SQL]] để xem thông tin về  các bảng đã được xóa
```sql
show tables dropped in <schema_name>
```

# Cloning table

## Deep clone

Deep clone sẽ copy tất cả thông tin bao gồm cả metadata và data files ra 1 chỗ khác với table cũ. Tức là với Deep clone, table mới sẽ keep được các thông tin như partition, index, ... nếu như nó có trong [[Delta Lake|delta table]].
```sql
CREATE TABLE <new_table_name> DEEP CLONE <table_name>
```

## Shallow clone
Shallow clone thì chỉ copy metadata của table sang 1 cái khác, nhưng vẫn trỏ về data file của bảng cũ. Vậy nên việc xóa data file của bất kì bảng nào cũng khiến bảng còn lại bị lỗi.

Cú pháp thì chỉ cần thay DEEP bằng SHALLOW trong lệnh tạo bảng.

Tuy nhiên, nếu ta insert thêm dữ liệu vào bảng gốc, thì nó sẽ không được xuất hiện trong bảng cloned. Bởi vì bảng mới sẽ clone từ 1 specific version của bảng gốc, và sẽ chỉ query được từ version đó. Các version khác của bảng gốc sau khi clone sẽ không ảnh đến bảng mới. Còn nếu chúng ta thực hiện các operation trên bảng mới, Databricks sẽ tạo ra các data file mới, dành riêng cho cloned table và sẽ MOR data lại với nhau.

> Test lại bằng `Describe history command`


# Delta table

## UPSERT
## MERGE