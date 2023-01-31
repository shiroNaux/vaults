---
---
# Abstract

Mục đích ban đầu của [[Kubernetes]] là để triển khai các stateless app. Nhưng sau đó nhờ sự hỗ trợ của cộng đồng, nhiều tính năng mới được phát triển. Storage tách riêng tài nguyên lưu trữ ra khỏi các tài nguyên tính toán. Nếu như không có storage, thì mỗi khi pod hay service bị kill, các thông tin được lưu trữ trong pods/servcies sẽ bị mất theo. Điều này chỉ thực sự phù hợp cho các stateless application. Storage có khả năng lưu trữ persistent data, khiến nó có thể được backup và phục hồi mỗi khi có sự cố liên quan đến pod.

## CSI
CSI stand for container storage interface. 

## Volume

Volume trong kubernetes cũng khá giống với volume trong [[Docker]], tuy nhiên volume của kubernetes có nhiều điểm nổi bật hơn. 
Về cơ bản thì volume là 1 directory, có thể chứa data bên trong. Mỗi pod muốn sử dụng volume thì phải mount volume đó vào 1 path trong file system của nó.

### Types of volumes

Kubernetes là 1 sản phẩm open source, có cung cấp interface cho storage -> có rất nhiều loại volume được các vendor họ tạo ra để kinh doanh các sản phẩm của mình, có thể kể đến các loại Volume như:
- AWS EBS CSI Migration, sẽ sử dụng [[AWS]] EBS để lưu trữ volume data.
- azureDisk CSI Migration, sử dụng [[Azure]] Disk để lưu trữu volume.
- cephfs
- ...

Ở đây, chúng ta chỉ nói đến những giải pháp free hoặc open source, có thể deploy ở bất kì đây (trên cloud hay on-premise) và có thể được sử dụng ở hiện tại ([[2023-01-31]]), chưa bị deprecated

#### emptyDir
emptyDir sẽ được tạo trước tiên khi ta tạo pod trên 1 node, và nó chỉ tồn tại khi pod đó tồn tại trên node đó. Các container trong cùng pod 

> Chú ý: emtyDir ở level node và pod. Khi pod bị crash thì emtyDir sẽ không mất đi. Chỉ khi pod bị remove khỏi node thì emtyDir mới bị xóa theo.

#### fc (fiber channel)
#### OpenStack CSI migration
#### downwardAPI
#### hostPath
#### iscsi
#### local
#### nfs
### Using subPath

## Persistent volumes
## Projected volumes
