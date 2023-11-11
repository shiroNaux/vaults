---
aliases: 
tags:
  - proxy
  - load_balancer
---
# Abstract

Reverse proxy là 1 loại proxy, về cơ bản thì nó đứng giữa client và ứng dụng, có chức năng forward request từ client về server.


Ngoài mục đích chính là để chuyển tiếp request, reverse proxy còn kiêm thêm các chức năng khác như
- Load balancing
- [[SSL]] encryption
- Caching
- Protec from attack

Reserve proxy còn được coi là 1 trong các loại server (máy chủ) -> thường sẽ sử dụng các máy chủ chuyên biệt cho mục đích này.


# Reverse proxy vs forward [[proxy]]

Điểm khác biệt lớn nhất nằm ở chức năng. Forward proxy nằm trước client. Nó sẽ chấp nhận request từ client và gửi nó đến server. Như vậy mục đích là để ẩn đi thông tin client

![[Pasted image 20231112035600.png]]

Reverse proxy thì ngược lại, nó đứng trước server

![[Pasted image 20231112040037.png]]

Nhằm ngăn client gửi request trực tiến đến server
# Reverse proxy vs [[Load Balancing|Load balancer]]

> A Reverse Proxy accepts a request from a client, forwards it to a server that can fulfill it, and returns the server’s response to the client.


- Load balancing
- Security
- Perfomance Acceleration
	- SSL Termination
	- Caching

=> Có thể thấy 1 Reserve proxy có bao gồm chức năng của 1 lòa balancer

# References
1. [A Primer on Proxies (cloudflare.com)](https://blog.cloudflare.com/a-primer-on-proxies/)
2. [What is a reverse proxy? | Proxy servers explained | Cloudflare](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)
3. 