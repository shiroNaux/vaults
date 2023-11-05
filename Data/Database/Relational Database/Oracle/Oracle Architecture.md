---
aliases: 
tags:
  - Oracle
  - Database
---

> A database system is basically a computer-based record-keeping system. The main use of the database is that the same collection of data should serve as many applications as possible.

# Core Concept

## SID
When connecting to an Oracle instance, you have the option to connect using either the SID or the Service Name. The SID (System Identifier) is a unique name that identifies your instance/database. On the other hand, the Service Name is the TNS (Transparent Network Substrate) alias that you give when you remotely connect to your database. This Service Name is recorded in the tnsnames.ora file on your clients and can be the same as the SID or any other name you want.

In short, the SID is the unique name of your instance/database, while the Service Name is an alias used when connecting to your database remotely. Is there anything else you would like to know? 😊


# Oracle Database Architecture

Thông thường khi nhắc tới database thì ta sẽ hiểu là 1 software được run trên 1 server. Tuy nhiên, Oracle có 1 số định nghĩa hơi khác so với cách hiểu thông thường

Oracle database được chia làm 2 thành phần chính
- Database system: chính là hệ thống các files lưu trữ, chứa dữ liệu của database
- Instance: Bao gồm các tài nguyên Memory và Compute để database có thể hoạt động
-> Có thể thấy kiến trúc của Oracle database đã tách ra storage và compute riêng -> Khả năng scale giống các cloud native hiện đại

![[Pasted image 20230819212051.png]]

## Database system

Database system ám chỉ hệ thống các files lưu trữ để database có thể hoạt động. Nó bao gồm các loại files như sau:
- Data files -> Hiển nhiên là để lưu trữ data mà người dùng đưa vào [[Oralce]]
- Control files -> Chứa metadata của database như Database name, LSN, vị trí các file data, ...
- Redo log files ->

![[Pasted image 20230819213546.png]]

Ngoài ra Oracle còn sử dụng 1 số loại file khác để có thể hoạt động, bao gồm:
- Parameter files -> Chứa các thông tin cần để initialization database. Parameter files cũng được chia ra 2 loại
	- Server parameter files (SPFILE): 
- Pasword files -> yepp, lưu trữ password của user để đăng nhập vào hệ thống
- Network files:
- Backup files: ->
- Archived redo log files ->

### Logical storage structure
## Instance

Oracle database instance là process run trên server.
Có một số lưu ý về Oracle instance:
- Instance có thể được khởi động mà không cần database files -> Điều này có thể hiểu được là: Khi lần đầu khởi chạy database, chưa hề có database files nào, ta cần dùng chính instance để tạo ra database.
- 1 Instance chỉ có thể access 1 database (files system) khi chạy. Quá trình Instance sử dụng database files nào chính là <u>mount database files</u>
- Multiple instacne có thể access cùng 1 database -> khá dễ hiểu nếu như sử dụng NFS hay các network file system để làm nơi lưu trữ data

Giống như các process thông thường khác, Oracle instance có 2 thứ để hoạt động:
- Memory
- Compute

![[Pasted image 20230819220909.png]]

### Memory structures

#### SGA

SGA stand for System Global Area

SGA được allocate khi startup Instance và release khi stop -> Chính là bộ nhớ được cấp phát ban đầu cho Instance

#### PGA

PGA stand for Program Global Area

Điểm khác biệt giữa SGA và PGA chính là PGA là private memory
### Background processes

Các Background process bao gồm:
- PMON
- SMON
- DBWn
- CKPT
- LGWR
- ARCn
- MMON
- LREG

# Multitenenant Architecture

Với Multitenant Architecture, 1 Oracle Database được coi là 1 CDB(Container database). CDB có thể chứa Application container và PDB(Plugable database)

![[Pasted image 20230914130225.png]]

Nếu không áp dụng multitenant Architecture, mỗi Oracle instance chỉ có thể tương tác với 1 Database cùng lúc. Do đó, bắt đầu từ phiên bản 12, Oracle đã sử dụng Multitenanet Architecture để cho phép sử dụng nhiều database hơn, khiến nó trở nên giống với hầu hết các [[DBMS]] khác. 
## CDB

CDB là level cao nhất của Oracle database. Nó sẽ chứa >= 1 PDB hoặc Application Container. Mỗi 1 Oracle instacne chỉ có thể có 1 CDB (Có thể hiểu CDB chính là Oracle Database)

CDB chỉ là container nên nó không chứa bất kì thông tin cũng như dữ liệu nào của User hay Application. Tuy nhiên nó sẽ chứa hầu hết các thông tin khác phục vụ cho hoạt động của Oracle Database: controlfiles, datafiles, undo, tempfiles, redo logs etc.

CDB sẽ chứa các containers:
- 1 CDB Root: 
- 1 System container
- >= 0 Application container
- >= 0 PDB
- 1 Seed PDB
## PDB

## Application container
# References
1. [Oracle Database 19c Technical Architecture](https://www.oracle.com/webfolder/technetwork/tutorials/architecture-diagrams/19/pdf/db-19c-architecture.pdf)
2. [An Overview of Oracle Database Architecture (oracletutorial.com)](https://www.oracletutorial.com/oracle-administration/oracle-database-architecture/)
3. [Microsoft PowerPoint - Oracle_Architecture.ppt (cern.ch)](https://indico.cern.ch/event/36804/attachments/731758/1003980/oracleArchitecture.pdf)
4. [Introduction to Multitenant Administration (oracle.com)](https://docs.oracle.com/en/database/oracle/oracle-database/21/multi/introduction-to-the-multitenant-architecture.html#GUID-267F7D12-D33F-4AC9-AA45-E9CD671B6F22)
5. [Oracle Architecture - GeeksforGeeks](https://www.geeksforgeeks.org/oracle-architecture/)
6. [Oracle Database Instance](https://docs.oracle.com/en/database/oracle/oracle-database/21/cncpt/oracle-database-instance.html#GUID-B3A8DB74-211A-453C-8B73-B61824DC56F6)
7. [Oracle Multitenant Architecture - Dot Net Tutorials](https://dotnettutorials.net/lesson/oracle-multitenant-architecture/)
8. [ORACLE-BASE - Multitenant : Overview of Container Databases (CDB) and Pluggable Databases (PDB)](https://oracle-base.com/articles/12c/multitenant-overview-container-database-cdb-12cr1)
9. [CDBs and PDBs (oracle.com)](https://docs.oracle.com/en/database/oracle/oracle-database/21/cncpt/CDBs-and-PDBs.html#GUID-5C339A60-2163-4ECE-B7A9-4D67D3D894FB)
10. [Oracle Database Architecture with Diagram - Dot Net Tutorials](https://dotnettutorials.net/lesson/oracle-database-architecture/)
11. [Oracle Database Storage Structures - Dot Net Tutorials](https://dotnettutorials.net/lesson/oracle-storage-structures/)