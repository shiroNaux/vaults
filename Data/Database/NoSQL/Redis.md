---
tags:
  - Redis
  - Database
  - NoSQL
aliases:
  - Redis
  - redis
---
# Introduction

Redis là 1 [[Database]] được chia vào loại [[NoSQL]]. Model lưu trữ data của là Key-Value. Redis thường được sử dụng cho [[Caching]] bởi vì Redis lưu trữ data trên [[RAM|memory]] cho nên tốc độ truy vấn sẽ nhanh hơn. Tuy nhiên nó cũng sẽ gây ra vấn đề là: Dữ liệu sẽ bị mất đi nếu Server bị down or crash. Để khắc phục điều này, Redis cũng có cơ chế backup data ra [[Hard disk|disk]] để persistant data.

# Architecture

# Storage

# Redis Cluster

Redis Cluster là phương pháp để Redis có thể scale horizontaly (linear scale). Cũng giống như các phương pháp scaling khác, 1 Cluster Redis sẽ bao gồm nhiều máy khác nhau, dữ liệu sẽ được partition đều ra cho các máy để lưu trữ và xử lý. Khi có nhu cầu scale thì chỉ cần thêm hoặc giảm số lượng máy trong cluster.



## Features
- Automatically split data among multiple nodes.
- Vẫn có thể hoạt động bình thường trong Th có 1 số máy failure.

## Master-Replica Model

Model này giúp Redis Clutser đạt được HA. Mỗi máy trong cụm Redis sẽ đảm nhận 1 trong 2 vai trò: Master hoặc Replica. Dữ liệu sẽ được ghi vào cả Master và Replica. Khi Master không thể sử dụng được thì Replica sẽ được promote thành Mater và phục vụ Client.

Dữ liệu đồng bộ giữa Master và Replica theo cơ chế [[Asynchronous|async]] cho nên tính Consistency sẽ không được đảm bảo.
# Redis Sentinal

# Comparision

## Redis vs Alternative

### Redis vs DragonflyDB

## Redis Sentinal vs Redis Cluster

# References
