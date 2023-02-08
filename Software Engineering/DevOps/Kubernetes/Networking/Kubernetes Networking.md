---
---
# Abstract
---
Có 1 số điểm cần lưu sau về network tromg [[Kubernetes]]
- Mỗi Pod sẽ được gắn 1 [[IP]] unique trên toàn bộ cluster -> về mặt network, các pod sẽ được coi như là VMs hay physical hosts.
- Các pods có thể connect với tất cả các pod khác trong cluster mà không cần [[NAT]].
- agents trên các node có thể connect tới mọi pod trên node đó

[[IP]] trong Kubernetes có scope mức pod -> mọi container trong pod đều có chung IP  Và thực tế là các container trong cùng pod share chung network namespaces (bao gồm IP, và MAC). -> Coi pod là 1 VM thì container chính là các process trong VM đó. Mô hình này được gọi là "IP-per-pod" model.

Với mô hình Kubernetes networking như trên, nó có thể giải quyết được 4 vấn đề sau:
- Các container trong cùng 1 pod connect với nhau qua loopback
- Cluster chịu trách nhiệm quản lý kết nối giữa các Pods
- Để các traffic từ bên ngoài có thể access tài nguyên trong cluster, có thể sử dụng Service [[API]]
- Ngoài ra, [[Kubernetes Service|service]] cũng dùng để thực hiện các kết nối giữa các thành phần trong cluster

---

### Container to container network
### Pod to pod network
### Pod to service network
### Internet to service network

---

# References
1. [Kubernetes Networking Fundamentals – techbeatly](https://www.techbeatly.com/kubernetes-networking-fundamentals/)
2. [The life of a DNS query in Kubernetes — NsLookup learning](https://www.nslookup.io/learning/the-life-of-a-dns-query-in-kubernetes/?ref=architecture-notes)
3. 