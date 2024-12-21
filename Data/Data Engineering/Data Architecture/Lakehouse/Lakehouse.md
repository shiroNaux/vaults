---
tags:
  - lakehouse
  - DataArchitecture
---

# Lakehouse

Đối với các database thông thường thì có thể bao gồm các thành phần:
- 


![[lakehouse.png]]

Để lưu trữ được metadata của table, các table open format sẽ tạo thêm mục thư mục ẩn trong folder chứa data, cụ thể như sau:

![[table_format_structure.png]]



# Notion

### Merge on Read

The **Merge-on-Read** strategy optimizes write performance by logging updates separately rather than rewriting base files immediately. Changes are stored in delta logs, and when data is queried, these logs are merged with the base files on-the-fly.

#### Advantages
- Tốt cho các workload cần update hoặc thay đổi dữ liệu nhiều, thường xuyên
- Phù hợp với các ứng dụng cần xử lý real time
#### Disadvantages
- Sẽ chậm hơn COW do phải merge dữ liệu trước khi gửi cho client

### Copy on Write
In the **Copy-on-Write** approach, any updates or modifications to a dataset result in the creation of new versions of the data files. Instead of altering existing files, COW writes changes to new files, which ensures that the original data remains intact until the new version is finalized.

#### Advantages
- 
#### Disadvantages
- Write chậm do phải tạo file mới, copy dữ liệu rồi mới append

|                   | CoW                                      | MoR                                     |
| ----------------- | ---------------------------------------- | --------------------------------------- |
| Update Method     | Use Cases                                | Appends changes to delta files          |
| Read Performance  | Fast                                     | Can be slower due to merging            |
| Write Performance | Slower, especially for small updates     | Fast                                    |
| Complexity        | Simpler                                  | More complex                            |
| Use Cases         | Read-heavy workloads, infrequent updates | Write-heavy workloads, frequent updates |


### Write-Audit-Publish


## Catalogs

Catalogs là phần mềm lưu trữ metadata

# Reference

1. https://lakefs.io/hudi-iceberg-and-delta-lake-data-lake-table-formats-compared/
2. [Apache XTable: Interoperability for Hudi, Iceberg, Delta | Medium](https://dipankar-tnt.medium.com/onetable-interoperability-for-apache-hudi-iceberg-delta-lake-bb8b27dd288d)