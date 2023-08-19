> A database system is basically a computer-based record-keeping system. The main use of the database is that the same collection of data should serve as many applications as possible.

# Oracle Database Architecture

Thông thường khi nhắc tới database thì ta sẽ hiểu là 1 software được run trên 1 server. Tuy nhiên, Oracle có 1 số định nghĩa hơi khác so với cách hiểu thông thường

Oracle database được chia làm 2 thành phần chính
- Database system: chính là hệ thống các files lưu trữ, chứa dữ liệu của database
- Instance: Bao gồm các tài nguyên Memory và Compute để database có thể hoạt động
-> Có thể thấy kiến trúc của Oracle database đã tách ra storage và compute riêng -> Khả năng scale giống các cloud native hiện đại

![[Pasted image 20230819212051.png]]

## Database system
## Instance
# Multitenenant Architecture
