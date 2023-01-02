---
---
# Abstract

Concurrency control là việc kiểm soát việc nhiều tiến trình khác nhau sử dụng chung tài nguyên (tranh chấp tài nguyên). Đây là 1 vấn đề quan trọng trong computer science, [[Operating System|os]] nói chung và [[Database]] nói riêng. Đối với các database, concurrency control là việc đảm bảo tính thống nhất dữ liệu cho các câu [[SQL|query]] khi rất nhiều các thao tác đọc ghi được thực hiện đồng thời với nhau, đảm bảo ko có conflict giữa các operations. Nó đặc biệt quan trọng trong các [[Relational Database]] khi mà các [[Database]] loại này yêu cầu rất cao về tính [[ACID]].

Nếu không đảm bảo được khả năng CC, có 1 số điều sau có thể xảy ra:
- Lost update:
- Dirty read: 
- hantom read
- Non-repeateable read
- Incorrect summary issues

# Concurrency Control trong database

## Concurrency Control Protocols
---

### Lock-Based protocols
Đối với protocol loại này, các resource sẽ bị khóa loại khi có 1 [[process]] hay transaction sử dụng tài nguyên. Các porcess/transaction khác phải đợi đến khi lock được release thì mới tiếp cận được data. Các phương pháp lock này được phân loại thành:
- Shared lock
- Exclusive lock
- Update Lock
- Intent lock
- Bulk update

#### Lock level


#### Lock type (Lock model)
##### Shared Lock (S)
Shared lock hay còn được gọi là read-only lock. Đối với protocol này, resource sẽ bị khóa đối với write operations, còn các read operation vẫn sẽ được thực hiện.

##### Exclusive Lock (X)
Đối với exclusive lock, chỉ có duy nhất 1 transaction/process được thực hiện read, write. Hay chỉ có duy nhất 1 transaction được chiếm exclusive lock

##### Update Lock (U)
##### Intent Lock (I)
##### Schema Lock (Sch)
##### Bulk Update (BU)

#### Các protocol (Các phương pháp thuộc lock based protocols)
---
##### Simplistic Lock 
Đây là protocol đơn giản nhất thuộc lock-based. Khi 1 transaction sử dụng data, tất cả cá transaction khác sẽ phải đợi đến khi transaction này hoàn thành rồi mới tiếp cận được data.

##### Pre-claiming Lock
Protocol loại này sẽ kiểm tra xem các tài nguyên cần dùng để thực hiện transaction là những gì. Sau đó nó sẽ phải đợi cho đến khi các tài nguyên này được __grant__

##### Two phase locking protocols

### Timestamp-Based protocols
### Validation-Based protocols

## Concurency Control Mechanism
---

Các concurrency control method được chia vào 3 loại chính

### Pessimistic Concurrency Control

Pessimistic CC sẽ block tất cả các transaction khác khỏi việc truy cập data để tránh xung đột, Loại CC này thường được sử dụng trong các điều kiện số lượng concurrency operation lớn. Điểm yếu của cách xử lý này là: việc lock data lại có thể dẫn đến việc giảm hiệu năng của database.

### Optimistic Concurrency Control

Optimistic được sử dụng trong điều kiện mà conccurency operation xảy ra ít. Nó sẽ không block các transaction khác khỏi việc truy cập data. Tuy nhiên sau khi commit và trước khi data được ghi vào [[Hard disk]] thì sẽ có thêm 1 bước đó là kiểm tra lại version của

#### Phase of optimistic concurrency control
- Begin: Ghi lại timestamp tại thời điểm bắt đầu transaction
- Modify: Thực hiện các lệnh trong transaction
- Validate: Kiểm tra xem dữ liệu có bị thay đổi bởi process nào khác không trong quá trình thực hiện transaction. Việc kiểm tra này dựa vào thời gian đã ghi lại ở bước 1.
- Commit/Rollback: Sau khi kiểm tra, nếu có conflict -> rollback còn không thì sẽ thực hiện operations.

#### Multiversion Concurrency control
Multiversion CC có thể coi là 1 phiên bản năng cấp của OCC. Nó được sử dụng trong hầu hết các [[Database]] thông dụng hiện nay.

### Semi-optimistic Concurrency Control

# Concurrency control in [[PostgreSQL]]

Multiversion CC là cơ chế xử lý concurrency operations chính của [[PostgreSQL]]
# References

1. [Concurrency control - Wikipedia](https://en.wikipedia.org/wiki/Concurrency_control)
2. [Optimistic concurrency control – Wikipedia](https://en.wikipedia.org/wiki/Optimistic_concurrency_control)
3. [Multiversion concurrency control – Wikipedia](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)
4. [Concurrency Control in DBMS - Scaler Topics](https://www.scaler.com/topics/dbms/concurrency-control-in-dbms/)
5. [DBMS Concurrency Control: Timestamp & Lock-Based Protocols (guru99.com)](https://www.guru99.com/dbms-concurrency-control.html)
6. [Exclusive lock và Shared lock - Viblo - Dat Bui](https://viblo.asia/p/010-exclusive-lock-va-shared-lock-924lJjn0lPM)
7. [database replication - Optimistic vs Multi Version Concurrency Control - Differences? - Stack Overflow](https://stackoverflow.com/questions/5751271/optimistic-vs-multi-version-concurrency-control-differences)
8. [PostgreSQL: Documentation: 13: Chapter 13. Concurrency Control](https://www.postgresql.org/docs/13/mvcc.html)