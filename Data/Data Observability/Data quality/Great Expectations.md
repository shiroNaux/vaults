---
---
## Data source

- Data source là các tập hợp các config, gồm 2 loại:
	- Dữ liệu được lưu ở đâu: local, S3, Azure, ..., dạng nào: file, SQL, memory. -> nằm trong data connector config
	- Các thức dùng để tiến cận dữ liệu: đọc bằng pandas, spark, sqlAlchemy, ... -> phần này config trong mục execution_engine