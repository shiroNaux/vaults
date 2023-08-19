> A database system is basically a computer-based record-keeping system. The main use of the database is that the same collection of data should serve as many applications as possible.

# Oracle Database Architecture

Thông thường khi nhắc tới database thì ta sẽ hiểu là 1 software được run trên 1 server. Tuy nhiên, Oracle có 1 số định nghĩa hơi khác so với cách hiểu thông thường

Oracle database được chia làm 2 thành phần chính
- Database system: chính là hệ thống các files lưu trữ, chứa dữ liệu của database
- Instance: Bao gồm các tài nguyên Memory và Compute để database có thể hoạt động
-> Có thể thấy kiến trúc của Oracle database đã tách ra storage và compute riêng -> Khả năng scale giống các cloud native hiện đại

![[Pasted image 20230819212051.png]]

## Database system

Database system ám chỉ hệ thống các files lưu trữ để database có thể hoạt động. Nó bao gồm các loại files như sau:
- Data files: -> hiển nhiên là để lưu trữ data mà người dùng đưa vào [[Oralce]]
- Control files: 
- Redo log files:

![[Pasted image 20230819213546.png]]

Ngoài ra Oracle còn sử dụng 1 số loại file khác để có thể hoạt động, bao gồm:
- Parameter files:
- Pasword files:
- Network files:
- Backup files ->
- Archived redo log files ->
## Instance

Oracle database instance là process run trên server.
Có một số lưu ý về Oracle instance:
- Instance có thể được khởi động mà không cần database files -> Điều này có thể hiểu được là: Khi lần đầu khởi chạy database, chưa hề có database files nào, ta cần dùng chính instance để tạo ra database.
- 1 Instance chỉ có thể access 1 database (files system) khi chạy. Quá trình Instance sử dụng database files nào chính là <u>mount database files</u>
- Multiple instacne có thể access cùng 1 database -> khá dễ hiểu nếu như sử dụng NFS hay các network file system để làm nơi lưu trữ data


# Multitenenant Architecture
