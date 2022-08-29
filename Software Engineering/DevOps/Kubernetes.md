---
---
# Kubernetes

## Why?


# Architecture

- Written in [[go lang]]

## Master-Slave architecture

A Kubernets cluster is composed of many machines, each one called node and has it's roles and duties. Kubernetes follow Master-Slave architecture. It has a centralized controller called master node or control plane (***minimal cluster***).

![[../../_images/0 4NDfryrEmVpo2UOp.png]]

This server acts as a gateway by exposing an [[API]] for users and clients, health checking other servers, deciding how best to split up and assign work (known as “scheduling”). 
The other machines in the cluster are designated as **nodes** -> accepting and running workloads using local and external resources. To help with isolation, management, and flexibility, Kubernetes runs applications and services in [[container|containers]], so each node needs to be equipped with a container runtime (like [[Docker]] or rkt). The node receives work instructions from the master server and creates or destroys containers accordingly, adjusting networking rules to route and forward traffic appropriately

## Control plane components

### 1. kube-apiserver
- Là end point để user hoặc các services truy cập các tài nguyên trên Kubernetes cluster. Là điểm trung gian giữa người dùng và cluster -> thành phần quan trọng nhất
- Nó cung cấp [[RESTful]] [[API]] -> là web services. 
- Không chỉ là cầu nối giữa người dùng và cụm Kubernetes, nó còn là trung gian giữa các thành phần trong chính cluster
- Người dùng tương tác với [[API]] server thông qua [[API]] hoặc [[CLI]]
### 2. etcd
- A distributed key-value store ([etcd.io](etcd.io)). Dùng để lưu trữ các dữ liệu phục vụ cho hoạt động của cluster. Để đảm bảo an toàn cho hệ thống, etcd chỉ có thể truy cập vào etcd thông qua [[API]] server
### 3. kube-controller-manager
### 4. cloud-controller-manager

# References
1. https://kubernetes.io/docs/concepts/architecture/
2. https://itnext.io/how-to-level-up-your-kubernetes-game-96f8f7ea50b9
3. https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes