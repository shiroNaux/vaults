# Postgres replication


## Nguyên lý:
- Postgres


![[../../../../_images/postgres_replication.jpg]]


## Khái niệm

	1. Falover
	2. Hot standby


## Cách thực hiện

1. Streaming Replication

	- Bước 1: tạo user, chỉnh sửa các config:
	  
		```
		CREATE USER {user_name} WITH REPLICATION PASSWORD '{password}';
		```
	-  Bước 1: Backup database bằng pg_basebackup. Việc này sẽ backup toàn bộ server bao gồm data và các config. Thư mục chứa dữ liệu backup phải empty. Ngoài backup, chạy câu lệnh pg_basebackup sẽ thêm 1 file standby để config standby server.
	  
		Syntax: 

		``` bash
		pg_basebackup -h {hostname} -U {user_name} -D {backup_folder_path} -Fp -Xs -P -R
		```
	
		Xem thêm các tham số khác với:

		``` bash
		pg_basebackup --help
		```
	- Bước 2: Khởi động server standby với tham số $PGDATA= vị trí thư mục đã backup ở bước trước
		- Tìm và sửa $PGDATA trong file service.
	- Bước 3: Config trạng thái đồng bộ:
		- Sửa giá trị trong mục: REPLICATION
	
2. Logical replication
	- 