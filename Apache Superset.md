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

1. Tất các các [[container]] đều chờ 2 services: [[Database|database]] và [[redis]]
2. Servcie tiếp theo là `superset-init`: -> khởi động, tạo các nội dung cần thiết (metadata [[database]]) để chạy superset
3. Tất cả các [[container]] đều có entrypoint là file `docker-bootstrap.sh`, mỗi container sẽ truyền argument thích hợp với nhiệm vụ của nó khi chạy.
4. 