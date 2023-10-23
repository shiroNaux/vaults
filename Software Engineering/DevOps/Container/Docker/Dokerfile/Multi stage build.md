# Introduction

[[Docker]] multi satge build là 1 phương pháp giúp mở rộng khả năng build image của Docker
# Usage



Trong [[Dockerfile]] ta sẽ có thể define nhiều `FROM` instruction khác nhau. Mỗi `FROM` instruction sẽ đại diện cho 1 image cụ thể sẽ được build

Ví dụ:
```Dockerfile
FROM golang:1.21
WORKDIR /src
COPY <<EOF ./main.go
package main

import "fmt"

func main() {
  fmt.Println("hello, world")
}
EOF
RUN go build -o /bin/hello ./main.go

FROM scratch
COPY --from=0 /bin/hello /bin/hello
CMD ["/bin/hello"]
```


##### Đặt tên cho stage

# Benefits

1. Smaller image
	- 
2. Buld faster
3. Less vulnerabilities

# References
1. [Learn Multi-Stage Builds Easy With Examples - Docker Development Tips & Tricks - YouTube](https://www.youtube.com/watch?v=vIfS9bZVBaw)
2. [Multi-stage builds | Docker Docs](https://docs.docker.com/build/building/multi-stage/)
3. [Multi-stage | Docker Docs](https://docs.docker.com/build/guide/multi-stage/)
4. 