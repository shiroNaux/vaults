---
---
# Introduction

Kubernetes service là 1 cách để có thể access vào các tài nguyên trong [[Kubernetes]] cluster, hoặc là các tài nguyên trong cùng 1 cụm có thể connect tới nhau

Trong Kubernetes cluster, mỗi Pod được gắn 1 với 1 [[IP]] tuy nhiên với Deloyment thông thường thì sẽ có nhiều hơn 1 pod -> do đó nếu 1 pod die và bị restart lại thì IP của nó có thể bị thay đổi. Kubernetes service cũng sẽ giải quyết vấn đề này.

Định nghĩa về services:
> In Kubernetes, a Service is an abstraction which defines a logical set of Pods and a policy by which to access them (sometimes this pattern is called a micro-service)

Service trong Kubernetes là 1 [[REST]] object, cũng giống như pod hay các resources khác. Mỗi service khi được tạo ra, sẽ được gán kèm với 1 địa chỉ [[IP]] gọi là `cluster IP`.

Giao thức mặc định hỗ trợ khi dùng service là [[TCP]]

# References
1. [Service | Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/)
2. 

