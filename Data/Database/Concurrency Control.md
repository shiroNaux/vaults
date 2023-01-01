---
---
# Abstract

Concurrency control là việc kiểm soát việc nhiều tiến trình khác nhau sử dụng chung tài nguyên (tranh chấp tài nguyên). Đây là 1 vấn đề quan trọng trong computer science, [[Operating System|os]] nói chung và [[Database]] nói riêng. Đối với các database, concurrency control là việc đảm bảo thống nhất dữ liệu cho các câu [[SQL|query]] khi rất nhiều các thao tác đọc ghi được thực hiện đồng thời với nhau. Nó đặc biệt quan trọng trong các [[Relational Database]] khi mà các [[Database]] loại này yêu cầu rất cao về [[ACID]].

# Concurrency Control trong database

## Concurrency Control Protocols

### 1. Lock-Based protocols
### 2. Two phase locking protocols
### 3. Timestamp-Based protocols
### 4. Validation-Based protocols

## Concurency Control Mechanism

### 1. Optimistic Concurrency Control
### 2. Pessimistic Concurrency Control
### 3. Semi-optimistic Concurrency Control

# References

1. [Concurrency control - Wikipedia](https://en.wikipedia.org/wiki/Concurrency_control)