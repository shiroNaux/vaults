---
alias:
  - OLTP
  - Online transactional proccessing
---

# Online transactional proccessing

-  OLTP systems typically facilitate and manage **transaction**-oriented applications
- OLTP thường được dùng để chỉ các hệ thống cần có sự phản hồi ngay lập tức với user requests. 1 ví dụ nổi bật là máy ATM.
- Hầu hết các hệ thống hiện nay đều là OLTP và đều xoay quanh **transaction**

# Trait
- OLTP yêu cầu: high throughput and và insert- or update-intensive đối với database. Ngoài ra các ứng dụng dạng này thường sẽ phải đáp ứng số lượng người dùng cùng một thời điểm rất lớn -> Yêu cầu 1 hệ thống OLTP phải đảm bảo thêm các yếu tố sau:

	- *Availability*
	- *Speed*
	- *Concurrency*
	- *Recoverability*

- OLTP is typically contrasted to [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing "Online analytical processing") (online analytical processing), which is generally characterized by much more complex queries, in a smaller volume, for the purpose of business intelligence or reporting rather than to process transactions. Whereas OLTP systems process all kinds of queries (read, insert, update and delete), OLAP is generally optimized for read only and might not even support other kinds of queries. OLTP also operates differently from [[../Data Engineering/Data Proccessing/Batch Proccessing|batch proccessing]] and [[../../Grid Computing|grid computing]].
- In addition, OLTP is often contrasted to [[OLEP]], which is based on distributed event logs to offer strong consistency in large-scale heterogeneous systems. Whereas OLTP is associated with short atomic transactions, OLEP allows for more flexible distribution patterns and higher scalability, but with increased latency and without guaranteed upper bound to the processing time
- In large applications, efficient OLTP may depend on sophisticated transaction management software (such as [CICS](https://en.wikipedia.org/wiki/CICS "CICS")) and/or [database](https://en.wikipedia.org/wiki/Database "Database") optimization tactics to facilitate the processing of large numbers of concurrent updates to an OLTP-oriented database.

- For even more demanding decentralized database systems, OLTP brokering programs can distribute transaction processing among multiple computers on a network. OLTP is often integrated into service-oriented architecture (SOA) and Web services.

- Online transaction processing (OLTP) involves gathering input information, processing the data and updating existing data to reflect the collected and processed information. As of today, most organizations use a database management system to support OLTP. OLTP is carried in a client-server system.
- Online transaction process concerns about concurrency and atomicity. Concurrency controls guarantee that two users accessing the same data in the database system will not be able to change that data or the user has to wait until the other user has finished processing, before changing that piece of data. Atomicity controls guarantee that all the steps in a transaction are completed successfully as a group. That is, if any steps between the transaction fail, all other steps must fail also.

# Design consideration

## Rollback segments
- Rollback segments are the portions of database that record the actions of transactions in the event that a transaction is rolled back. Rollback segments provide read consistency, rollback transactions, and recovery of the [[database]] 
## Clusters
- A cluster is a schema that contains one or more tables that have one or more columns in common. Clustering tables in a database improves the performance of [[SQL|join]] operations.
## Discrete transactions
- A discrete transaction defers all change to the data until the transaction is committed. It can improve the performance of short, non-[[../../Distributed system|distributed]] transactions
## Block size
- The data block size should be a multiple of the operating system's block size within the maximum limit to avoid unnecessary I/O
## Buffer cache size
 - [[SQL]] statements should be tuned to use the database buffer cache to avoid unnecessary resource consumption
## Dynamic allocation
- Dynamic allocation of space to tables and rollback segments
## Transaction proccessing monitors and the multi-threaded server
- A transaction processing monitor is used for coordination of services. It is like an operating system and does the coordination at a high level of granularity and can span multiple computing devices
## Partition
- Partition use increases performance for sites that have regular transactions while still maintaining availability and security
## Database tunning
- With database tuning, an OLTP system can maximize its performance as efficiently and rapidly as possible

# Distinction

## 1. [[OLAP]]
- See in [[OLAP]]

## 2. [[OLEP]]
- 
# References
1. [Online transaction processing - Wikipedia](https://en.wikipedia.org/wiki/Online_transaction_processing)
