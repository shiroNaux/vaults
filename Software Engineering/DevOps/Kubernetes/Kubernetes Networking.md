---
---
# Abstract
---
Có 1 số điểm cần lưu sau về network tromg [[Kubernetes]]
- Mỗi Pod sẽ được gắn 1 [[IP]] unique trên toàn bộ cluster -> về mặt network, các pod sẽ được coi như là VMs hay physical hosts.
- Các pods có thể connect với tất cả các pod khác trong cluster mà không cần [[NAT]].
- agents trên các node có thể connect tới mọi pod trên node đó

[[IP]] trong Kubernetes có scope mức pod -> mọi container trong pod đều có chung IP  Và thực tế là các container trong cùng pod share chung network namespaces (bao gồm IP, và MAC). -> Coi pod là 1 VM thì container chính là các process trong VM đó. Mô hình này được gọi là "IP-per-pod" model.


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