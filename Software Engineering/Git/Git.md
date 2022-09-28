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
-> Lệnh `git add` thực chất chính là lấy content của từng file đã thay đổi rồi hash lại thành `object` và từ tất cả các thay đổi đó -> tạo thành `tree object`. Object này bao gồm những sự thay đổi của commit.
- The `git commit` takes those staged changes and creates trees pointing to all of the new blobs, then creates a new commit object pointing to the new root tree. Finally, `git commit` also updates the current branch to point to the new commit
- Hình bên dưới mô tả quá trình tạo ra các object khi ta thực hiện 1 thay đổi trong file `README.md` và thực hiện commit
![[../../_images/Git/gitdatabase5.png]]
- We can do slightly more complicated queries based on object data. Using `git log --pretty=format:<format-string>`, we can make custom queries into the commits by pulling out “columns” such as the object ID and message, and even the committer and author names, emails, and dates
- There are also some prebuilt formats ready for immediate use. For example, we can get a simple summary of a commit using `[git log --pretty=reference -1 <ref>](https://git-scm.com/docs/git-log#_pretty_formats)`. This query parses the commit at `<ref>` and provides the following information:

	- An abbreviated object ID.
	- The first sentence of the commit message.
	- The commit date in short form.
- Ví dụ
```bash
$ git log --pretty=reference -1 378b51993aa022c432b23b7f1bafd921b7c43835 378b51993aa0 (gc: simplify --cruft description, 2022-06-19)
```

# Compressed object storage: packfiles
- Khi vào trong thư mục `.git/objects`, điều đầu tiên mà ta nhận ra đó là: có rất nhiều thư mục có tên là 2 kí tự hexa. Bên trong những thư mục này là các file có tên là chuỗi các kí tự hexa. Các file này gọi là `loose object`. Có thể thấy là 2 kí tự đầu tiên của các files này chính là tên của thư mục chứa chúng. Những files này đã được nén lại nên không dùng các công cụ thông thường để xem được nội dung của chúng.
- Cách lưu trữ trên không được hiệu quả cho lắm khi mà số lượng object tăng cao, hay lưu trữ nhiều phiên bản khác nhau của cùng 1 file -> sinh ra nhiều objects => ___loose objects là gì___
- Git sử dụng `.git/objects/pack` để lưu trữ các object. Các file trong này được tạo ra bằng cách nhóm các object lại với nhau và rồi nén lại
- Cách nén các `objects`
![[../../_images/Git/gitdatabase6.png]]
Hình bên dưới mô tả việc `packed` các objects lại với nhau theo dạng đơn giản nhất
	- Các file này không chứa `object id` mà chỉ chứa content -> nếu muốn tìm kiếm thì phải decompress và hash từng object để có được object id, rồi so sánh chúng với object id input để tìm ra được kết quả
	- The pack-index file stores the list of object IDs in lexicographical order so a quick binary search is sufficient to discover if an object ID is in the packfile, then an _offset_ value points to where the object’s data begins within the packfile -> cách hoạt động giống với index trong các [[../../Data/Database/Relational Database/Relational Database|database]] 
	- Ngoài ra còn có 1 _fanout table_ chứa 256 entries (tương ứng với 2 ký tự hexa đầu tiên) -> tách nhỏ các objects để tìm kiếm nhanh hơn -> cách hoạt động của các partitions
# References
1. https://github.blog/2022-08-29-gits-database-internals-i-packed-object-store/
2. https://github.blog/2022-08-30-gits-database-internals-ii-commit-history-queries/
3. https://github.blog/2022-08-31-gits-database-internals-iii-file-history-queries/
4. https://github.blog/2022-09-01-gits-database-internals-iv-distributed-synchronization/
5. https://github.blog/2022-09-02-gits-database-internals-v-scalability/