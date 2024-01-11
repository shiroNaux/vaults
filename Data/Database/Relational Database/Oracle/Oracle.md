# Highlight Features

## Query language
##### Dynamic SQL

Dynamic [[SQL]] là 1 tính năng của [[Oracle]], cho phép người dùng viết các câu [[SQL|query]] dưới dạng generic, và rồi các câu query đó sẽ được render lúc runtime. Nó tương tụ như các phương pháp templating.

Có 2 cách để viết dynamic SQL trong PL/SQL:
- PL/SQL có sẵn tính năng hỗ trợ viết Dynamic [[SQL]]
- Sử dụng `DBMS_SQL` package, là 1 API để build run dynamic SQL statements
###### References
1. [PL/SQL Dynamic SQL (oracle.com)](https://docs.oracle.com/en/database/oracle/oracle-database/21/lnpls/dynamic-sql.html#GUID-7E2F596F-9CA3-4DC8-8333-0C117962DB73)
2. [DYNAMIC SQL (viblo.asia)](https://viblo.asia/p/dynamic-sql-RnB5pNAYZPG)

##### SELECT FOR UPDATE

Giống như các [[SQL]] khác, câu lệnh `SELECT FOR UPDATE` dùng để lock lại row từ câu lệnh `SELECT`. Lệnh này thường dùng để đảm bảo dữ liệu từ lệnh `SELECT` sẽ không bị thay đổi trong quá trình thực hiện _TRANSACTION_
