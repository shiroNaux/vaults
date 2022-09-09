# Git
---
---

# Abstract
- Được viết bằng [[C]], [[C++]], [[Python]], ...
- Ban đàu được tạo ra để hỗ trợ cho quá trình phát triển [[Linux]] kernel, sau đó phát triển thành 1 tiêu chuẩn để collaboration trong phát triển phần mềm
- Theo một số cách hiểu

> Git is the distributed [[database]] at the core of your engineering system.

- Các điểm giống nhau giữa git và [[database]]
	- Dữ liệu được lưu trên disk -> persistence
	- Sử dụng query để truy vấn dữ liệu. Cách thức mà git hay database lưu trữ dữ liệu đều nhằm mục đích tối ưu cho truy vấn, và các thuật toán tìm kiếm đều tận dụng tối đa khả năng của cấu [[Data structure|trúc dữ liệu]]
	- Với hệ thống distributed, có sự giao tiếp vào thống nhất giữa các node
- Nhưng git vẫn có một số điểm khác biệt so với các [[database]] thông thường
	- Git thường dùn để lưu trữ các file code -> dung lượng rất nhỏ, nên mọi thay đổi đều có thể được lưu lại 
	- Với các database, thông thường chúng là các long-live [[process]] và sử dụng 1 lượng lớn [[RAM]] để lưu trữ và xử lý dữ liệu. Nhưng git thì không, Git lưu dữ liệu ra file rồi xử lý trực tiếp trên các file đó, và chạy bằng các short-lived process

# Architecture
---
## Concept

- Git lưu trữ dữ liệu trong thư mục `.git` đặt cùng cấp với _root_ level của repository

### 1. Git object
- Git object là khái niệm cơ bản của git. Chúng là _atom_ của git repo, và các object khi kết hợp cùng nhau sẽ tạo ra các đối tượng khác trong git
- các thông tin về object được lưu tại `.git/objects`. Ví dụ:
``` bash
$ ls .git/objects/ 
01 34 9a df info pack 

$ ls .git/objects/01/ 
12010547a8990673acf08117134bdc181bd735 

$ls .git/objects/pack/ multi-pack-index pack-7017e6ce443801478cf19006fc5499ba1c4d2960.idx pack-7017e6ce443801478cf19006fc5499ba1c4d2960.pack pack-9f9258a8ffe4187f08a93bcba47784e07985d999.idx pack-9f9258a8ffe4187f08a93bcba47784e07985d999.pack
```

- Thư mục này được gọi là _object store_ hay `content-addressable data store` tức là nó có thể truy xuất thông tin dựa vào [[hash]] của nội dung lưu trữ (***can retrieve the contents of an object by providing a hash of those contents***) -> object store có cấu trúc giống 1 bảng mà có 2 cột `Object ID` và `Object content`. `Object Id` chính là  giá trị [[hash]] của content và hoạt dộng như khóa chính.

![[../../_images/Git/gitdatabase2.png]]

- Nhưng trên thực tế, các yêu cầu đối với git là truy vấn `content` bằng cách cung cấp `Id` -> Do đó Git có một số cách lưu trữ dữ liệu để là việc đó
- Đầu tiên git cho phép tạo name pointer để `references` tới `Object Id` trong bảng trên. Các `references` này được lưu trữu trong thư mục `.git/refs/`, và thư mục này cũng có cấu trúc riêng để phù hợp với mục đích