---
---
# Abstract

# Replica
# Sharding

Sharding là 1 cách mà MongoDB sử dụng để scale theo chiều ngang. Sharding trong MongoDB cũng giống với các [[Database]] khác, nó phân phối dữ liệu ra nhiều server khác nhau, mỗi server giữ 1 phần dữ liệu (được gọi là shard). Mỗi khi có yêu cầu query dữ liệu, MongoDB sẽ 

## Architecture

1 MongoDB sharding cluster bao gồm cá thành phần sau:
- shard: Là các node/server chứa data, thực hiện các chức năng của 1 MongoDB server thông thường. Các shard thường được deploy dưới dạng replica
- mongos: act as router. Các client sẽ kết nối đến mongos, và mongos sẽ route các yêu cầu đến các shard và nhận kết quả từ các shard rồi trả về client
- config servers: Lưu trữ metadata và các config cho hoạt động của cả cluster.

![[sharded-cluster-production-architecture.png]]

1 Cluster MongoDB tiêu chuẩn dùng trong production bao gồm các thành phần sau:
- Config servers là 1 replica set gồm 3 node
- 1 hoặc 2 mongos instance
- Các shard trong cluster được deploy dưới dạng replica gồm 3 node

### Shard

## Sharding key

# RAC

# References
