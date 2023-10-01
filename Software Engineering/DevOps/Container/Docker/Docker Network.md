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
Đây là netwrok default. 
> In terms of networking, a bridge network is a Link Layer device which forwards traffic between network segments. A bridge can be a hardware device or a software device running within a host machine's kernel.

Docker bridge netwrok thuộc loại phần mềm.

Các container sẽ được add vào trong default bridge netwrok nếu không được chỉ định. Các containẻ trong netwrok sẽ được cung cấp 1 địa chỉ [[Internet protocol|IP]] riêng trong network này -> Số container sẽ bị limt theo số địa chỉ IP. Các container trng cùng 1 network có thể connect với nahu thông qua IP hoặc service_name (Không phải hostname). Default là IPv4, đương nhiên là có thể sử dụng Ipv6 nếu được config.

Mục đích chính của bridge là để isolate docker network với host machine network. Khi docker hoạt động, nếu kiểm tra network thông qua lệnh `ipconfig` ta sẽ thấy 1 số interface đặc biệt

- docker0 -> interface của default bridge network
- br-xxxxx -> interface của các container hoặc compose đang có trên Docker

Tuy nhiên, mọi network bên trong container nếu muốn đi ra ngoài host machine sẽ đi qua docker0 bất kể là container đó sử dụng network là gì (điều này giải thích tại sao có thể dùng docker0 ip cho mọi container để thay thế cho host machine)
## host

## overlay

## ipvlan

## macvlan

## none

Ngoài ra còn 1 số driver plugin do cộng đồng phát triển (tuy nhiên không khuyến khích sử dụng). Chúng ta có thể cài hoặc tự phát triển các plugin này.

# Netwroking in Docker compose



# References
1. [Packet filtering and firewalls | Docker Docs](https://docs.docker.com/network/packet-filtering-firewalls/)
2. 