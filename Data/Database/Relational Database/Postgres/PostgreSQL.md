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
	- INSTEAD OF: -> chỉ được sử dụng với views và row-level only. 
Mặc định thì trigger sẽ được thực thi trong cùng 1 transaction với statement kích hoạt trigger -> điều này đảm bảo tính consistency cho database. Khi mà trigger hoặc statement bị lỗi -> rollback lại cả 2. Riêng đối với các AFTER trigger, mặc định thì trigger function sẽ thực thi ngay sau khi statement được hoàn thành, tuy nhiên có thể tùy chỉnh để trigger function sẽ chạy ở cuối của transaction (constraint trigger). 

Một trường hợp đặc biệt đó là: Đối với statement loại `INSERT ON CONFLICT DO UPDATE` có thể kích hoạt đồng thời nhiều trigger theo thứ tự:
	- BEFORE INSERT
	- BEFORE UPDATE
	- AFTER UPDATE
	- AFETR INSERT
Một trường hợp cần lưu ý nữa là: Nếu 1 statment move record từ partition này sang 1 partition khác, nó có thể sẽ kích hoạt đồng thời các trigger:
	- 