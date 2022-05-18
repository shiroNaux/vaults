# Go programming language

# Go packages
- Các files trong cùng 1 folder được go ngầm coi là trong cùng 1 package. Sử dụng các hàm trong cùng package không cần phải khai báo import
- Nếu hàm cần dùng nằm trong 1 folder khác -> cần import package

# Go module
- Go module là tập các package
- Thông tin về module được lưu ở file `go.mod` ở root.
- File `go.mod` bao gồm các thông tin:
	- _module path_
	- _dependency requirements_

## Create new module

``` go
	go mod init {{module_name}}
```

