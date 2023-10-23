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