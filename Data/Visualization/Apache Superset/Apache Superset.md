---
---
# Apache Superset
---
# Setup Dev Environment
---
# Deploy Docker

Các services trong docker compose của superset
- Superset app -> chạy app
- database (có thể dùng **extrenal**)
- redis (có thể dùng **extrenal**)
- Superset worker -> **celery** worker (không phải web server worker), dùng để chạy query [[async]], tránh timeout của http
- Superset init -> chạy lần đầu để tạo schema, import data và meta database
- Superset worker beat -> check health của worker

Ngoài ra, môi trường dev còn có thêm 1 số servcies khác
- Superset node (là nodeJs, khác với services trong [[../../../Software Engineering/DevOps/Kubernetes/Kubernetes]] là node compute)
- Superset web-socket

## Cách hoạt động

1. Tất các các [[container]] đều chờ 2 services: [[Database|database]] và [[redis]]
2. Servcie tiếp theo là `superset-init`: -> khởi động, tạo các nội dung cần thiết (metadata [[database]]) để chạy superset. Entrypoint là `docker-init.sh`.
	1. Trong khi superset init các điều kiện cần thiết, có 1 config là: `CYPRESS_CONFIG` -> chạy integration test (sử dụng file config đặt trong test folder). Nên đặt là `false` trong production
	2. Chạy lệnh upgrade database, nếu schema đã có sẵn -> upgrade, nếu trống -> tạo mới chema và insert các data cơ bản (`role`, `permission`,...)
	-> Hiện tại đang thấy file compose của superset không có option set password của `admin`, trong file `docker-init.sh` đang fix cứng giá trị này là: `admin` -> Nếu muốn thay đổi giá trị này -> sửa file `docker-init.sh`
	3. Setup `admin` user sử dụng lệnh `superset fab`
	> ⚠️⚠️ Hiện tại đang thấy file compose của superset không có option set password của `admin`, trong file `docker-init.sh` đang fix cứng giá trị này là: `admin` -> Nếu muốn thay đổi giá trị này -> sửa file `docker-init.sh`

3. Tất cả các [[container]] đều có entrypoint là file `docker-bootstrap.sh`, mỗi container sẽ truyền argument thích hợp với nhiệm vụ của nó khi chạy.
	1. App container: chạy superset app
		> The “webserver” that ships with flask (e.g. “app.run()”) is really great for local development. And that’s about it. It isn’t designed to take the abuse that a normal webserver receives

		Do đó khi chạy superset trong production nên dùng `app-unicorn`, và `app` chỉ nên chạy trên local development
	2. Superset worker
	3. Superset worker beat
4. Ngoài ra có một số [[container]] dùng cho developemnt
	1. Superset node: Chạy trên node image, ***mount volume chung*** với các image chạy app -> mỗi khi code có thay đổi thì run build lại (dùng `webpack --watch`)
	2. Superset websocket
3. Các container đều sử dụng file `.env` hoặc `.env-non-dev`. Trong các file này, các giá trị được sử dụng là `DATABASE_DB` chứ ko phải là `POSTGRES_DB`. Ngoài ra các container đều mount volume `pythonpath_dev`. Nó chứa các scripts python. Trong đó có `superset_config.py` dùng để overwrite các config mặc định. Tuy nhiên, các files trong `pythonpath_dev` sẽ được mount vào container trong `docker-compose.yaml` file. Còn nếu deploy bằng [[../../../Software Engineering/DevOps/Kubernetes/Kubernetes]] sẽ phải sử dụng các `secrets` hay `configMap`
---
# Deploy Kubernetes
- Khi dùng helm chart của superset, `superset_config.py` được mount từ `secrets`: `superset_config`, chứa một số config mặc định. Có thể thay đổi các config của superset bằng cách thêm các giá trị trong file `values.yaml` của [[helm]] chart.
---
# Build a custom Image
- Superset cung cấp sẵn `Dockerfile` để dev có thể tự tạo image cho các mục đích riêng.
- `Dockerfile` của superset sử dụng **multi-stage build** -> dễ dàng chỉnh sửa các thành phần của image cuối cùng theo từng stage
- Docker image chỉ tạo folder `pythonpath` chứ không đưa bất cứ file nào vào -> Nếu muốn overwrite lại các config -> mount volume hoặc dùng secret, env