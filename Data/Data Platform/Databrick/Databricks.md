---
aliases:
---
# Dictionary
- Control plane:
- Data plane
- All-Purpose cluster:
- Job Cluster
- SQL Warehouse:
- Photon:
- Catalog:
- [[Delta Lake]]:
- Notebook:
- Magic command: Cũng giống như magic command trong Jupyter Notebook, được bắt đầu bằng ***`%`*** để thực hiện các câu lệnh đặc thù như cài package(%pip install)
- Transaction log
- Managed table: Là các table được quản lý (bao gồm cả data và metadata) bởi chính Databricks
- External table: Databricks sẽ có thông tin meta data của các table này, nhưng data file sẽ không được quản lý bởi Databricks
- Schema enforcement
- Delta Live Table
- Streaming table
- **Materialized table**:
- **Auto loader**: Là 1 tính năng của Databricks cho phép auto load dữ liệu từ các nguồn Cloud storage. Databricks sẽ tự động nhận biết các file dữ liệu mới và ingest chúng về Databricks.
- DLT: Delta live table


# Databricks Architecture

## Account
Trong Databricks, account là đơn vị cấu trúc cấp bậc cao nhất. Bên dưới account sẽ bao gồm các workspace là đơn vị logical để phân chia môi trường làm việc cho các workload khác nhau. Account còn dùng để quản lý users, group, service principle, SSO, ... nói chung là các vấn đề liên quan đến quyền truy cập dữ liệu và tính năng của Databricks. Account cũng thực hiện quản lý billing và Unity catalog metastore.

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


## References
1. [High-level architecture | Databricks on AWS](https://docs.databricks.com/aws/en/getting-started/high-level-architecture)

# Cluster Structure