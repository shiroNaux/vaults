---
---
# Kubernetes

## Why?


# Architecture
## Basic info
- Written in [[go lang]]

## Master-Slave architecture

A Kubernets cluster is composed of many machines, each one called **node** and has it's roles and duties. Node có thể là VM hay physical machine. Kubernetes follow Master-Slave architecture. It has a centralized controller called master node or control plane (***minimal cluster***).

![[0 4NDfryrEmVpo2UOp.png]]

This server acts as a gateway by exposing an [[API]] for users and clients, health checking other servers, deciding how best to split up and assign work (known as “scheduling”). 
The other machines in the cluster are designated as **nodes** -> accepting and running workloads using local and external resources. To help with isolation, management, and flexibility, Kubernetes runs applications and services in [[Container|containers]], so each node needs to be equipped with a container runtime (like [[Virtualization/Container/Docker]] or rkt). The node receives work instructions from the master server and creates or destroys containers accordingly, adjusting networking rules to route and forward traffic appropriately

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
- Ty có nhiều loại controller khác nhau, và mỗi loại lại được chạy bởi 1 [[Process]] riêng, nhưng chúng đều được compiled chung trong 1 binary
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
- Ghi lại các time-series metrics về các container, lưu lại trong central [[database]]
- Cung cấp UI để access data
### 4. Cluster-level Logging
- Mục đích: Lưu lại log của container ra central log store with search/browsing interface

## Object
### 1. Pod
### 2. 

## Node
### 1. Node name 
- Node được phân biệt bới __name__ -> 2 Node không được trùng tên
- Ngoài ra Kubernetes còn coi 2 resource có tên giống nhau là cùng 1 object
- Trong trường hợp có 2 node cùng tên: Kubernetes sẽ coi đó là 2 node mà có cùng state (network settings, root disk contents, ...) và các attributes như node labels -> có thể dẫn inconsistencies nếu 1 trong 2 node thay đổi các trạng thái mà không đổi tên
- Nếu cần phải thay thế node hoặc thực hiện các thay đổi có sự ảnh hưởng lớn, thì đầu tiên, node hiện tại phải được loại bỏ khỏi [[API]] server và sau đó re-added sau khi đã thay đổi

### 2. Management
Có 2 cách để add 1 node vào cluster
- kubelet trên node sẽ tự register node đó với control plane
- Người sử dụng sẽ add thủ công

#### Self-registration
- When the kubelet flag `--register-node` is true (the default), the kubelet will attempt to register itself with the API server. This is the preferred pattern, used by most distros.
#### Manual node administration
- Sử dụng `kubectl` để add node 1 cách thủ công
### 3. Heartbeats
- Là tín hiệu được node gửi đến control plane để xác định tình trạng hiện tại của node
- Nếu node đang ở trạng thái failed thì cluster sẽ thực hiện các actions cần thiết
- Để update heartbeats của node có 2 phương thức:
	- updates `.status` của node
	- Thực hiện *lease* objects trong namespace `kube-node-lease`. Mỗi Node sẽ có 1 _lease_ object tương ứng
- *kubelet* có trách nhiệm update heartbeat của Node
	- Update `.status` của node được thực hiện theo _interval_ với giá trị mặc định 5 phút 1 lần(việc update luôn được thực hiện kể cả giá trị `.status` có tahy đổi hay không). 
	- Đối với _lease_ object, kubelet sẽ thực hiện mỗi 10s. Nếu update thất bại, kubelet sẽ retries với cơ chế [[exponential backoff]] bắt đầu với 200ms và giới hạn ở 7s
### 4. Node controller
- Là thành phần nằm trong _control plane_, có nhiệm vụ quản lý các Nodes
- Gán [[CIDR]] cho Node khi cần
- Đảm bảo trạng thái của các Nodes trên control plane giống như trạng thái của các machine trên cloud.Nếu sử dụng trên các nền tảng cloud, khi có 1 Node nào đó unavailable, node controller sẽ hỏi provider xem machine tương ứng với Node còn hoạt động được ko. nếu không -> xóa Node khỏi cluster
- Nhiệm vụ tiếp theo của node controller là giám sát trạng thái của node. Node cotroller sẽ check trạng thái của node mỗi 5s.
	- Nếu node rơi vào trạng thái unreachable -> update giá trị `.status` của node thành `unknown`
	- Nếu node vẫn ỏ trạng thái unreachable sau 1 khoảng thời gian (mặc định là 5 phút), thì node controller sẽ thực hiện *API-initiated eviction* đối với mọi pod trong node đó để loại bỏ chúng khỏi cluster
#### Rate limit on eviction
- Số lượng pod bị evict khỏi node sẽ bị giới hạn (giá trị mặc định là 0.1 -> không loại bỏ quá 1 pod khỏi node trong 10s)
- Nếu Kubernetes được triển khai trên nhiều availability zone thì giá trị này sẽ thay đổi theo từng zone kèm theo nhiều điều kiện khác (số node, tỉ lệ node chết, ...)
### 8. Graceful node shutdown & None graceful shutdown
- Graceful node shutdown là 1 tính năng của Kubernetes, nó đảm bảo các pod trên node được shutdown đúng cách 
- Tính nưng này phụ thuộc vào [[systemd]](sử dụng [[systemd]] inhibit block) để delay node shutdown
- Trong 1 số trường hợp pod shutdown nhưng không được ghi nhận bởi _kubelet's Node Shutdown Manager_, do đó pod có thể bị stuck ở trạng thái terminating -> sử dụng tính năng Non-gracefule shutdown để giải quyết vấn đề này
### 10. Swap memory management
- Từ phiện bản 1.22, Kubernetes hỗ trợ node sử dụng swap. Config theo từng node. Khả năng sử dụng swap sẽ bị phụ thuộc vào config `swapBehavior` và [[cgroup]] của node.
- Tuy nhiên để đảm bảo hiệu năng cho cluster -> không nên sử dụng swap

## Communication between node and control plane
### 1. Node to control plane
- Kubernetes sử dụng cơ chế __hub-and-spoke__ để giao tiếp.
- Các Node giao tiếp với Control plane thông qua [[API]] Server sử dụng [[HTTPS]](port 443), kết nối đên [[API]] Server. Tất cả các node phải có root certificate để giao tiếp với control plane. Ngoài ra các components của control plane cũng giao tiếp với API server qua port [[HTTPS]]
### 2. Control plane to node
- Kết nối từ control plane đến node được chia làm 2 loại
	- Kết nối từ [[API]] Server đến kubelet. 
		- Mục đích của kết nối này là:
			- Fetching logs for pods
			- Attaching (usually through `kubectl`) to running pods
			- Providing the kubelet's port-forwarding functionality
		- Các connections này sẽ trỏ tới kubelet's [[HTTPS]] endpoint. Mặc định là [[API]] server sẽ không verify kubelet's certificate, do đó các kết nối này có thể bị tấn công theo kiểu [[man-in-the-middle]] và đương nhiên là nó không an toàn để connect giữa các thành phần với nhau qua public Internet. Tuy nhiên, có thể config cho kubelet sử dụng root certificate để secure connections. Hoặc cũng có thể dùng cách khác _SSH Tunnel_
		- Ngoài ra, nên sử dụng kubelet [[Authentication]] and/or [[Authorization]] để gia tăng khả năng bảo mật
	- Kết nối từ API Server đến nodes, pods, services
		- Các kết nối kiểu này theo mặc định là sử dụng [[HTTP]], nên nó dễ bị tấn công -> không nên sử dụng ở môi trường `untrusted` hay Internet
		- Để các connections này sử dụng HTTPS, chỉ cần thay prefix `https` vào trước đường dẫn tới node, pod, hay service, tuy nhiên chúng sẽ không xác nhận endpoint certificate nên vẫn được coi là không an toàn để chạy trong `untrusted` hay Internet

### 3. SSH Tunnel
- Đã bị deprecated
- Kubernetes hỗ trợ [[SSH]] Tunnel để bảo vệ các kết nối từ control plane tới nodes.
- [[API]] Server sẽ tạo SSH Tunnel tới mọi node và sau đó giao tiếp với các node hay kể cả kubelet thông qua Tunnel đã được thiết lập

### 4. Konnectivity service
- Là giải pháp thay thế cho [[SSH]] Tunnel khi mà nó đã bị deprecated
- Konnectivity là 1 service. Nó sẽ cung cấp [[Reserve Proxy]] ở level [[TCP]] để control plane kết nối tới các thành phần khác trong cluster
- Bao gồm 2 phần:
	- Konnectivity server ở control plane
	- Konnectivity agents nằm trong nodes
- The Konnectivity agents khởi tạo các connections tới Konnectivity server và maintain các connections này
- Sau đó mọi network traffics từ control plane tới node đều đi qua các connections đó

## Controller
- Implement [[control loop]] idea. Nó sẽ quan sát state của cluster, nếu phát hiện bất thường thì sẽ thực hiện các thay đổi để đưa cluster về desired state
- Mỗi object trong Kubernetes đều có spec mô tả desired state. Mỗi loại controller sẽ quan sát từng loại object mà nó phụ trách và có trách nhiệm đưa curent state của object về gần desired state nhất.Có 2 cách để thay đổi state của object:
	- Controller có thể tự thực hiện các thay đổi đối với object
	- Hoặc gửi message đến API Server để có thể có nhiều tác động hơn

- Kubernetes mặc định sẽ có 1 số built-in controller, được run trong `kube-controller-manager`
- Ngoài ra, có thể sử dụng các external-controller, hoặc tự viết controller

## Cloud controller manager
- Cloud controller manager hoạt động theo cơ chế _pligin_, điều đó cho phép Kubernetes hoạt động được với nhiều cloud provider khác nhau

### 1. Node controller
Node controller có nhiệm vụ update các Node khi mà các machine hay server được tạo ra trên cloud infrastructure. Node controller sẽ có những trách nhiệm sau:
	- Update Node tương ứng với server id
	- Annotating và labelling cho node tương ứng với các đặc điểm như region, resource (CPU, RAM, ...), ...
	- Gán Hostname và network address
	- Kiểm tra health của node. Nó sẽ suer dụng cloud provider [[API]] để kiểm tra các server và update vào trạng thái của node
### 2. Route controller
- Route controller sẽ config network trên hệ thống cloud để các thành phần trong cluster có thể giao tiếp với nhau dễ dàng và an toàn
### 3. Service controller
- Service controller sẽ tương tác với các dịch vụ của cloud provider như load balancer, IP, Packet filtering, ...và sử dụng chúng để hỗ trợ cho hoạt động của cluster.

#
##
### Annotations & Labels

Annotations in Kubernetes (K8s) are metadata used to express additional information related to a resource or object.
Annotations và labels hoạt động với cơ chế key-value pairs

Các 


| Annotations                                                  | Labels                                |
| ------------------------------------------------------------ | ------------------------------------- |
| Annotations are used to store data about the resource itself | Labels are used to identify resources |
|                                                              |                                       |


# References
1. [Concepts | Kubernetes](https://kubernetes.io/docs/concepts/)
2. [How To Level Up Your Kubernetes Game | by Piotr | ITNEXT](https://itnext.io/how-to-level-up-your-kubernetes-game-96f8f7ea50b9)
3. [An Introduction to Kubernetes | DigitalOcean](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes)
4. [inhibit (www.freedesktop.org)](https://www.freedesktop.org/wiki/Software/systemd/inhibit/)
5. 