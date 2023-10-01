---
aliases:
---
# Introduction

[[Docker]] có riêng 1 cơ chế về [[Virtualizatio|ảo hóa]] networking để phù hợp với các yêu cầu về isolation tài nguyên khi sử dụng [[container]].

Về cơ bản thì Docker network là 1 cơ chế cho phép người dùng kết nối các container với nhau thông qua network trong 1 mạng ảo của chính Docker. Cơ chế này giúp hoàn thiện khả năng ảo hóa của Docker.

Mỗi 1 container trong Docker có 