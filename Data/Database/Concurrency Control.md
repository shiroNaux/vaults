---
---
# Abstract

Concurrency control là việc kiểm soát việc nhiều tiến trình khác nhau sử dụng chung tài nguyên (tranh chấp tài nguyên). Đây là 1 vấn đề quan trọng trong computer science, [[Operating System|os]] nói chung và [[Database]] nói riêng. Đối với các database, concurrency control là việc đảm bảo tính thống nhất dữ liệu cho các câu [[SQL|query]] khi rất nhiều các thao tác đọc ghi được thực hiện đồng thời với nhau, đảm bảo ko có conflict giữa các operations. Nó đặc biệt quan trọng trong các [[Relational Database]] khi mà các [[Database]] loại này yêu cầu rất cao về tính [[ACID]].

Nếu không đảm bảo được khả năng CC, có 1 số điều sau có thể xảy ra:
- Lost update:
- Dirty read
- hantom read
- Non-repeateable read
- Incorrect summary issues

# Concurrency Control trong database

## Concurrency Control Protocols
---

### Lock-Based protocols
Đối với protocol loại này, các resource sẽ bị khóa loại khi có 1 [[process]] sử dụng tài nguyên.

#### Shared Lock (S)
Shared lock hay còn được gọi là read-only lock. Đối với protocol này, resource sẽ bị khóa đối với write operations, còn các read operation vẫn sẽ được thực hiện.

#### Exclusive Lock (X)
Đối với exclusive lock, chỉ có duy nhất 1 transaction/process được thực hiện read, write. Hay chỉ có duy nhất 1 transaction được chiếm exclusive lock

#### Simplistic Lock 
#### Pre-claiming Lock
#### Update Lock
#### Intent Lock
#### Schema Lock

#### Two phase locking protocols

### Timestamp-Based protocols
### Validation-Based protocols

## Concurency Control Mechanism
---

Các concurrency control method được chia vào 3 loại chính

### Pessimistic Concurrency Control

Pessimistic CC sẽ block tất cả các transaction khác khỏi việc truy cập data

### Optimistic Concurrency Control
#### Phase of optimistic concurrency contro
- Begin: Ghi lại timestamp tại thời điểm bắt đầu transaction
- Modify: Thực hiện các lệnh trong transaction
- Validate: Kiểm tra xem dữ liệu có bị thay đổi bởi process nào khác không trong quá trình thực hiện transaction. Việc kiểm tra này dựa vào thời gian đã ghi lại ở bước 1.
- Commit/Rollback

#### Multiversion Concurrency control
Multiversion CC có thể coi là 1 phiên bản năng cấp của OCC. Nó được sử dụng trong hầu hết các [[Database]] thông dụng hiện nay.

### Semi-optimistic Concurrency Control

# Concurrency control in [[PostgreSQL]]


# References

1. [Concurrency control - Wikipedia](https://en.wikipedia.org/wiki/Concurrency_control)
2. [Concurrency Control in DBMS - Scaler Topics](https://www.scaler.com/topics/dbms/concurrency-control-in-dbms/)
3. [DBMS Concurrency Control: Timestamp & Lock-Based Protocols (guru99.com)](https://www.guru99.com/dbms-concurrency-control.html)
4. [Exclusive lock và Shared lock - Viblo - Dat Bui](https://viblo.asia/p/010-exclusive-lock-va-shared-lock-924lJjn0lPM)