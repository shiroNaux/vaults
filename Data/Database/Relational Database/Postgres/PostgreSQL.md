# PosstgreSQL


# PL/pgSQL
### Trigger

Trigger trong PostgreSQL có thể được gắn với table, foreign table và cả view

Trigger trong PostgreSQL có thể được kịch hoạt theo theo 2 hướng:
	- per-row triggers (row-level) -> hàm trigger sẽ được gọi đối vỡi mỗi row bị ảnh hưởng bởi statement kích hoạt trigger
	- per-statement triggers (statement-level) -> hàm trigger chạy duy nhất một lần cho statement kích hoạt trigger
Các trigger cũng được phân loại dựa trên thời điểm mà hàm trigger thực thi so với thời điểm mà statement chạy
	- BEFORE: -> trigger chạy trước khi statement được thực thi
	- AFTER: -> ngược lại với BEFORE, trigger sẽ chạy sau khi statement đã hoàn thành
	- INSTEAD OF:
Mặc định thì trigger sẽ được thực thi trong cùng 1 transaction với statement kích hoạt trigger -> điều này đảm bảo tính consistency cho database. Đối với AFTE