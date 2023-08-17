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
Một trường hợp cần lưu ý nữa là: Nếu 1 statment move record từ partition này sang 1 partition khác, nó có thể sẽ kích hoạt 1 loạt các triggger trên cả partition cũ và partition mới (loằng ngoằng vl, đéo ai mà nhớ được 😒😒 -> đọc ở đây nè [PostgreSQL: Documentation: 15: 39.1. Overview of Trigger Behavior](https://www.postgresql.org/docs/current/trigger-definition.html))
Đối với các MERGE statement, các trigger được kích hoạt sẽ tùy thuộc vào kết quả của statement này.
##### Return
Các trigger cần return lại một số giá trị
- statement-level triggers: function luôn return NULL
- row-level triggers: function có thể return NULL hoặc 1 table row (a value of type `HeapTuple`). Tùy vào giá trị trả về mà sẽ có những thay đổi về giá trị input của statement(BEFORE trigger)
	- Nếu return NULL -> skip operation đối với row hiện tại (tức là sẽ không thực thi các lệnh INSERT, DELETE, UPDATE đối với row hiện tại nữa)
	- Nếu return trả về table row -> Các INSERT, UPDATE triggers sẽ lấy giá trị return từ trigger function để thực hiện các operation thay cho giá trị gốc (input của trigger)
-> Cần phải chú ý các giá trị RETURN của cá row-level BEFORE trigger
Với các AFTER trigger, giá trị return không quan trọng nên thường được để là NULL.
Nếu có nhiều trigger có được kích hoạt dựa trên cùng 1 event, các trigger này sẽ được thực hiện lần lượt theo alphabet. Và các giá trị return từ trigger trước sẽ là input cho các trigger phía sau. Nếu có 1 trong số các trigger này return NULL, toàn bộ các trigger phía sau sẽ không được thực hiện nữa.