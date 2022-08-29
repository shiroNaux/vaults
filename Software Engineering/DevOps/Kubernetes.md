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
- A distributed key-value store ([etcd.io](etcd.io)). Dùng để lưu trữ các dữ liệu phục vụ cho hoạt động của cluster. Để đảm bảo an toàn cho hệ thống, chỉ có thể truy cập vào etcd thông qua [[API]] server. Dữ liệu lưu trong etcd thường là dạng [[YAML]]
- etcd lưu trữ là **consistent**
### 3. kube-controller-manager
- Bao gồm nhiều loại:
	- Node controller: Responsible for noticing and responding when nodes go down.
	- Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
	- Endpoints controller: Populates the Endpoints object (that is, joins Services & Pods).
	- Service Account & Token controllers: Create default accounts and API access tokens for new namespaces.
- Mỗi loại lại có 1 chức năng riêng biệt là quản lý các chức năng của Kubernetes
- Ty có nhiều loại controller khác nhau, và mỗi loại lại được chạy bởi 1 [[process]] rieeng, nhưng chúng đều được compiled chung trong 1 binary
### 4. kube-scheduler
- The process that actually assigns workloads to specific nodes in the cluster is the scheduler. This service reads in a workload’s operating requirements, analyzes the current infrastructure environment, and places the work on an acceptable node or nodes.
- The scheduler is responsible for tracking available capacity on each host to make sure that workloads are not scheduled in excess of the available resources. The scheduler must know the total capacity as well as the resources already allocated to existing workloads on each server.

### 5. cloud-controller-manager

## Node components
- Các thành phần mà 1 node cần có, bao gồm cả control plane

# References
1. https://kubernetes.io/docs/concepts/architecture/
2. https://itnext.io/how-to-level-up-your-kubernetes-game-96f8f7ea50b9
3. https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes