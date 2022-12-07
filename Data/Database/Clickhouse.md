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




# References
1. [What Is ClickHouse? | ClickHouse Docs](https://clickhouse.com/docs/en/intro/)
2. 
