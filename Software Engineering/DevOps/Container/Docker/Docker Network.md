---
aliases:
---
# Introduction

[[Docker]] có riêng 1 cơ chế về [[Virtualizatio|ảo hóa]] networking để phù hợp với các yêu cầu về isolation tài nguyên khi sử dụng [[container]].

Về cơ bản thì Docker network là 1 cơ chế cho phép người dùng kết nối các container với nhau thông qua network trong 1 mạng ảo của chính Docker. Cơ chế này giúp hoàn thiện khả năng ảo hóa của Docker.

Mỗi 1 container trong Docker có thể coi như 1 Compute Engine đọc lập. Và đương nhiên nó sẽ có 1 số chức năng về network giống như các máy ảo hay máy vật lý

Khi cài đặt docker trên [[Linux]] có sử dựng firewalld, thì cũng sẽ 1 Network Interface được setup cho chúng ta, đó là docker interface. Nó giống như các interface khác và ta hoàn toàn có thể config interface này để Docker network hoạt động như ta mong muốn.

# Network Drivers

Để Docker netwrok hoạt động, ta cần có Netwrok drier. Các driver hoạt động theo cơ chế plugable. Hầu hết các driver được cài đặt mặc định cùng với Docker.

## bridge
## host

## overlay

##ipvlan

#mac

# Netwroking in Docker compose



# References
1. [Packet filtering and firewalls | Docker Docs](https://docs.docker.com/network/packet-filtering-firewalls/)
2. 