---
tags:
  - deployment
aliases:
---

![[deployment_strategy.jpg]]

---
### Blue-Green
Đặc điểm:
- Kiểu deploy này yêu cầu có  môi trường chạy song song với nhau.
	- Môi trường Blue
	- Môi trường Green
- 1 môi trường sẽ chạy version cũ, môi trường còn lại sẽ chạy version mới.
- Khi cần nâng cấp thì sẽ swithc từ môi trường blue -> green (thông qua [[load balancer]], [[DNS]], ...) Nếu có lỗi thì sẽ switch lại môi trường cũ

![[blue-green-deploy.png]]

**Pros**:
- Có thể đạt được Zero downtime
- Nếu xảy ra lỗi có thể dễ dàng rollback lại
- Do các môi trường là độc lập với nhau nên các thao tác như testing trên mỗi môi trường cũng sẽ không ảnh hưởng đến nhau

**Cons**:
- Yêu cầu nhiều tài nguyên do phải maintain cả 2 môi trường cùng lúc

### Canary

![[canary-deploy.png]]

### Bigbang
### Rolling

![[rolling-deploy.png]]

### A/B Testing


![[ab-testing-deploy.png]]

### Feature Flag

![[feature-flag-deploy.png]]


# References
1. [5 Must-Know Deployment Strategies - by Saurabh Dashora](https://newsletter.systemdesigncodex.com/p/5-must-know-deployment-strategies)