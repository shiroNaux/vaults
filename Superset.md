---
---
# Apache Superset

# Deploy Docker

Các services trong docker compose của superset
- Superset app -> chạy app
- database (có thể dùng **extrenal**)
- redis (có thể dùng **extrenal**)
- Superset worker -> ***???***
- Superset init -> chạy lần đầu để tạo schema, import data và meta database
- Superset worker beat -> check health của worker

Ngoài ra, môi trường dev còn có thêm 1 số servcies khác
- Superset node
- Superset web-socket

## Cách hoạt động

1. Tất các các [[container]] đều chờ 2 services
3. Entrypont của [[docker]] [[container]] là: `docker-bootstrap.sh`
4. 