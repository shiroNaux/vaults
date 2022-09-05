---
---
# Kubernetes

## Why?


# Architecture
## Basic info
- Written in [[go lang]]

## Master-Slave architecture

A Kubernets cluster is composed of many machines, each one called **node** and has it's roles and duties. Node có thể là VM hay physical machine. Kubernetes follow Master-Slave architecture. It has a centralized controller called master node or control plane (***minimal cluster***).

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
- Scheduler có nhiệm vụ giám sát available capacity trên các node trong cluster. Và mỗi khi nhận được workload’s operating requirements, nó sẽ phân tích, tính toán và đưa ra quyết định assign workload vào 1 hoặc nhiều node 1 một cách hợp lý sao cho không có node nào phải chạy vượt quá lượng tài nguyên mà node đó có.
- Các yếu tố ảnh hưởng tới quyết định của scheduler bao gồm
	- individual and collective resource requirements
	- hardware/software/policy constraints
	- affinity and anti-affinity specifications
	- data locality
	- inter-workload interference
	- deadlines

### 5. cloud-controller-manager
- cloud-controller-manager liên kết với các Cloud provider's [[API]]. Loại controller này chỉ xuất hiện khi sử dụng Kubernetes trên cloud (**SaaS hay IaaS**)
- cloud-controller-manager có nhiệm tách riêng các tài nguyên nằm trong cụm Kubernetes của nó ra khỏi các tài nguyên trên cloud khác không liên quan đến cluster -> cô lập cluster khỏi các thành phần khác
- Bao gồm các thành phần
	- Node controller: Checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
	- Route controller: Setting up routes in the underlying cloud infrastructure
	- Service controller: Creating, updating and deleting cloud provider load balancers

## Node components
- Các thành phần mà 1 node cần có, bao gồm cả control plane

### 1. kubelet
- là agent, run trên mọi node của cluster, có nhiệm vụ đảm bảo container được run trong pod
- kubelet sẽ nhận PodSpecs được cung cấp thông qua nhiều cách/cơ chế khác nhau và đảm bảo rằng các containers được mô tả trong PodSpecs running and healthy. 
- kubelet không quản lý các containers mà không được tạo bởi Kubernetes
### 2. kube-proxy
- is a network proxy that run trên mọi node trong cluster, implementing part of the Kubernetes Service concept.
- kube-proxy maintains network rules on nodes. Các network rules này quản lý các kết nối đến và đi khỏi cluster
- kube-proxy uses the [[OS]] packet filtering layer nếu có và có thể sử dụng. nếu không, kube-proxy forwards the traffic itself
### 3. Container runtime
 - container runtime is the software that is responsible for running containers
- hỗ tất cả các container runtime mà implement **Kubernetes CRI**, bao gồm:
	- containerd
	- CRI-O
## Addons
- Addons là các tài nguyên thứ cấp. Chúng sử dụng các services (DaemonSet, Deployment, ... được tạo nên bởi các tài nguyên cơ bản) để hình thành nên cá tính năng, tài nguyên mới của cụm Kubernetes
- Dưới đây là 1 số addons tiêu biểu
- Danh sách addons đầy đủ tại: [Installing Addons | Kubernetes](https://kubernetes.io/docs/concepts/cluster-administration/addons/)
### 1. DNS
- strictly required
- Cluster DNS là 1 DNS server của Kubernetes cluster, serves DNS records for Kubernetes services.
- Các container được tạo bởi Kubernetes sẽ có DNS server này trong list các DNS searches
### 2. Web UI (Dashboard)
- general purpose, web-based UI for Kubernetes clusters
- Cho phép người dùng quản lý cluster thông qua Web UI
### 3. Container Resource Monitoring
- Ghi lại các time-series metrics về các container, lưu lại trong central database
- Cung cấp UI để access data
### 4. Cluster-level Logging
- Mục đích: Lưu lại log của container ra central log store with search/browsing interface

## Node
### 1. Node name 
- Node được phân biệt bới __name__ -> 2 Node không được trùng tên
- Ngoài ra Kubernetes còn coi 2 resource có tên giống nhau là cùng 1 object
- Trong trường hợp có 2 node cùng tên: Kubernetes sẽ coi đó là 2 node mà có cùng state (network settings, root disk contents, ...) và các attributes như node labels -> có thể dẫn inconsistencies nếu 1 trong 2 node thay đổi các trạng thái mà không đổi tên
- Nếu cần phải thay thế node hoặc thực hiện các thay đổi có sự ảnh hưởng lớn, thì đầu tiên, node hiện tại phải được loại bỏ khỏi [[API]] server và sau đó re-added sau khi đã thay đổi

### 2. Manual Node administration
### 3. Node status
#### Address
#### conditions
#### Capacity & Allocatable



# References
1. https://kubernetes.io/docs/concepts/architecture/
2. https://itnext.io/how-to-level-up-your-kubernetes-game-96f8f7ea50b9
3. https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes