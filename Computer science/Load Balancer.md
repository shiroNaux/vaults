---
aliases:
---
# Introduction

> A load balancer distributes incoming client requests among a group of servers, in each case returning the response from the selected server to the appropriate client.

-> Load balancer có nhiệm vụ distribute request đến các server đích. Và đương nhiên là nó sẽ phải phân chia các request đến các server này 1 cách hợp lý bằng cách sử dụng các thuật toán cân bằng tải

Do đó Load balancer thường được sử dụng khi mà lượng request là nhiều và ta cần deploy nhiều services để scale ứng dụng


> ⚠️ Các ứng dụng như: Nginx, HAProxy hay traefik thường có ca2 chức năng của [[Reverse Proxy]] và Load Balancer

# Phân loại


# Load balancer algorithm

![[12dffcce-f231-48cc-915f-d53c0f8bce0c_3735x3573.jpg]]
