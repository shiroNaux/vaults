# JFrog Atifactory

# JFrog Container Registry
- JFrog có cung cấp bản miễn phí của artifactory, nhưng chỉ sử dụng được cho cho [[Docker Registry]] và helm
- Ngoài ra còn có một số registry khác cho JAVA

## Mục đích
Sử

## Why


## Concept
	- Trong JFrog có 3 loại repository:
		- Local: Các packages và images private với mục đích dùng trong internal
		- Remote: Các packages hay images không lưu trữ trên 1 registry khác và JFrog pull về để sử dụng.
		- Virtual: Chứa các packages hay images gồm cả 2 loại Local và Remote

## Sử dụng
	1. 

## Accept EULA bằng cmd

```
curl -XPOST -vu {user}:{password} http://{host}/artifactory/ui/jcr/eula/accept
```
