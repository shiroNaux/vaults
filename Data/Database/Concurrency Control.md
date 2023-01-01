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

### 1. Lock-Based protocols

Đối với protocol loại này, các resource sẽ bị khóa loại 
#### Shared Lock
#### Exclusive Lock
#### Simplistic Lock
#### Pre-claiming Lock

### 2. Two phase locking protocols
### 3. Timestamp-Based protocols
### 4. Validation-Based protocols

## Concurency Control Mechanism
---
### 1. Optimistic Concurrency Control
### 2. Pessimistic Concurrency Control
### 3. Semi-optimistic Concurrency Control

# References

1. [Concurrency control - Wikipedia](https://en.wikipedia.org/wiki/Concurrency_control)