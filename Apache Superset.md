---
---
# Apache Superset
---
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
2. Servcie tiếp theo là `superset-init`: -> khởi động, tạo các nội dung cần thiết (metadata [[database]]) để chạy superset. Entrypoint là `docker-init.sh`.
	1. Trong khi superset init các điều kiện cần thiết, có 1 config là: `CYPRESS_CONFIG` -> chạy integration test (sử dụng file config đặt trong test folder). Nên đặt là `false` trong production
	2. Chạy lệnh upgrade database, nếu schema đã có sẵn -> upgrade, nếu trống -> tạo mới chema và insert các data cơ bản (`role`, `permission`,...)
	-> Hiện tại đang thấy file compose của superset không có option set password của `admin`, trong file `docker-init.sh` đang fix cứng giá trị này là: `admin` -> Nếu muốn thay đổi giá trị này -> sửa file `docker-init.sh`
	3. Setup `admin` user sử dụng lệnh `superset fab`
	> :warning: Hiện tại đang thấy file compose của superset không có option set password của `admin`, trong file `docker-init.sh` đang fix cứng giá trị này là: `admin` -> Nếu muốn thay đổi giá trị này -> sửa file `docker-init.sh`

3. Tất cả các [[container]] đều có entrypoint là file `docker-bootstrap.sh`, mỗi container sẽ truyền argument thích hợp với nhiệm vụ của nó khi chạy.
	1. App container: chạy superset app
		> The “webserver” that ships with flask (e.g. “app.run()”) is really great for local development. And that’s about it. It isn’t designed to take the abuse that a normal webserver receives
		
		Do đó khi chạy superset trong production nên dùng `app-unicorn`, và `app` chỉ nên chạy trên local development
	2. Superset worker
	3. Superset worker beat
4. Ngoài ra có một số [[container]] dùng cho developemnt
	1. Superset node: Chạy trên node image, ***mount volume chung*** với các image chạy app -> mỗi khi code có thay đổi thì run build lại (dùng `webpack --watch`)
	2. Superset websocket
---
# Build a custom Image
- Superset cung cấp sẵn `Dockerfile` để dev có thể tự tạo image cho các mục đích riêng.
- `Dockerfile` của superset sử dụng **multi-stage build** -> dễ dàng chỉnh sửa các thành phần của image cuối cùng theo từng stage