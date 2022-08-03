---
---
# Introduction
Singer (singer.io) được định nghĩa là:

> The open-source standard for writing scripts that move data

Tức là Singer chỉ là 1 tiêu chuẩn (Specification) để việc chuyển data giữa các tất cả các nguồn cũng ngư đích. Tất cả các source và sink nếu tuân theo tiêu chuẩn này giúp việc vận chuyển data giữa các hệ thống trở nên dễ dàng.

# Các khái niệm
1. Tap: là source connector
2. Target: là sink connector

# Singer Specification

Tiêu chuẩn mà singer sử dụng rất đơn giản
- Tap sẽ đọc dữ liệu từ nguồn và xuất data ra stdout (console).
- Target sẽ đọc dữ liệu từ stdin (console) và ghi vào đích.
- ??? ***Chuẩn data format khi đưa ra console -> để target có thể biết đâu là 1 records***

# Sử dụng

- Set up python environment và cài đặt các tap, target cần thiết thông qua [[pip]]
- Chạy flow trong [[bash]] [[shell]] theo syntax

```
tap-source | target-destination
```

# References
1. [Singer | Open Source ETL](https://www.singer.io/)
2. [Singer (github.com)](https://github.com/singer-io)
3. [Singer Spec | MeltanoHub](https://hub.meltano.com/singer/spec)