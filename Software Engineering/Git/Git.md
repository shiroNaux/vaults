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
# Concepts

# Internal Architecture
---

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

- Nhưng trên thực tế, các thao tác sử dụng git thường là: lấy ra các file tại commit id nào? só sánh khác nhau giữa 2 commit,... -> tức là truy vấn `content` bằng cách cung cấp `Id` -> Git cần có một cách lưu trữ dữ liệu phù hợp cho việc đó. Và đó là ... (đéo phải fo len ti lô)
- Đầu tiên git có 1 khái niệm gọi là `references`, nó là 1 `named pointer` dùng để tham chiếu tới `Object id` trong __object database__. Các thông tin về `references` được lưu trong thư mục `.git/refs`. Có thể dùng hình ảnh dưới để mô tả quan hệ của `refs` và `objects`
![[../../_images/Git/gitdatabase3.png]]
- Với việc sử dụng `refs`, bây giờ người sử dụng có thể dễ dàng lấy thông tin của các `objetc` bằng cách sử dụng tags, name, ...
- Lấy ví dụ với bức ảnh trên (lấy từ [git code base](https://github.com/git/git)). Bắt đầu từ `refs` v2.37.0
	- `refs` `refs/tags/v2.37.0` point tới 1 annotated tag object. 1 annotated tag bao gồm 1 `reference` tới 1 object khác và 1 plain-text message.
	- Object được trỏ tới ở đây là 1 `commit`. 1 `commit` object là snapshot của worktree tại 1 thời điểm nào đó, kèm với 1 tham chiếu tới commit ngay trước nó (parent commit). Thông tin được lưu trong loại object này bao gồm:
		- links tới parent commit, ___root tree___
		- metadata: commit time, messages
	- That tag’s object references a commit object. A commit is a snapshot of the worktree at a point in time, along with connections to previous versions. It contains links to _parent commits_, a _root tree_, as well as metadata, such as commit time and commit message.
	-   That commit’s root tree references a tree object. A tree is similar to a directory in that it contains entries that link a path name to an object ID.
	-   From that tree, we can follow the entry for `README.md` to find a blob object. Blobs store file contents. They get their name from the tree that points to them.
![[../../_images/Git/gitdatabase4.png]]

## Object store query

Nếu trong các [[database]] thông thường, để lấy được dữ liệu thì chúng ta thường sử dụng [[SQL]], còn đối với git thì đó là [[../../Operating system/CLI|command line interface]].
Một số thao tác cơ bản để truy xuất dữ liệu của git
- Lấy nội dung của 1 `object` dựa vào Id của nó
``` bash
git cat-file <Object Id>
# Ví dụ:

# options
# -p: pretty -> dễ đọc
# -t: type của object

```

- Có thể tự insert data vào `object store` của git 🤔🤔:
```bash
git hash-object -w --stdin
Hello world
```
-> Lệnh `git add` thực chất chính là lấy content của tất cả các file trong repo rồi hash lại
# References
1. https://github.blog/2022-08-29-gits-database-internals-i-packed-object-store/
2. https://github.blog/2022-08-30-gits-database-internals-ii-commit-history-queries/
3. https://github.blog/2022-08-31-gits-database-internals-iii-file-history-queries/
4. https://github.blog/2022-09-01-gits-database-internals-iv-distributed-synchronization/
5. https://github.blog/2022-09-02-gits-database-internals-v-scalability/