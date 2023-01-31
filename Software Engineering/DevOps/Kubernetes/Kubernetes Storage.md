---
---
# Abstract

Mục đích ban đầu của [[Kubernetes]] là để triển khai các stateless app. Nhưng sau đó nhờ sự hỗ trợ của cộng đồng, nhiều tính năng mới được phát triển. Storage tách riêng tài nguyên lưu trữ ra khỏi các tài nguyên tính toán. Nếu như không có storage, thì mỗi khi pod hay service bị kill, các thông tin được lưu trữ trong pods/servcies sẽ bị mất theo. Điều này chỉ thực sự phù hợp cho các stateless application. Storage có khả năng lưu trữ persistent data, khiến nó có thể được backup và phục hồi mỗi khi có sự cố liên quan đến pod.

## CSI
CSI stand for container storage interface. 
## Volume
