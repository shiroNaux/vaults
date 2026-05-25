---
aliases:
---
# Glossaries

## Tasks
Properties:
- **Type:** Có các lựa chọn: Notebook, [[python]] script, [[dbt]], [[SQL]], [[Java|jar]], [[Delta live table]], ...
- **Source:** Cho phép chọn các tài nguyên có trên [[Databricks|databricks]] hoặc pull từ [[Git]] về.
- Cluster:
- Parameters:
- Notification:
- Retries
- Duration threshold:
- Condition: là 1 properties đặc biệt, chỉ đi kèm với Task type là if/else condition. Code bên trong codition sẽ dùng `{{ }}` để refer đến các giá trị


# Features

## Repair run

## Loop over task

Tính năng này cho phép người dùng loop lại các task bằng cách click chuột và chọn loop. Parameter inpt cho mỗi lần loop được specify bởi 1 array Input,