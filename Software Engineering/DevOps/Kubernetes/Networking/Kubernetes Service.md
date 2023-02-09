---
---
# Introduction

Kubernetes service là 1 cách để có thể access vào các tài nguyên trong [[Kubernetes]] cluster, hoặc là các tài nguyên trong cùng 1 cụm có thể connect tới nhau

Bởi vì trong Kubernetes, mỗi Pod được gắn 1 với 1 [[IP]] tuy nhiên với Deloyment thông thường thì sẽ có nhiều hơn 1 pod -> do đó dẫn đến việc 