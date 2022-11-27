---
---

## Data source

- Data source là các tập hợp các config, gồm 2 loại:
	- Dữ liệu được lưu ở đâu: local, S3, Azure, ..., dạng nào: file, SQL, memory. -> nằm trong data connector config
	- Các thức dùng để tiến cận dữ liệu: đọc bằng pandas, spark, sqlAlchemy, ... -> phần này config trong mục execution_engine
- Checkpoint: là **config**:
	- Xác định expectation với datasource -> tạo thành 1 checkpoint. Checkpoint ở đây không phải là 1 lần validate cụ thể mà chỉ là config.