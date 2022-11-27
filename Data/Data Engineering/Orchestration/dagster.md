---
---

# Introduction

# Architecture

## Concepts
### Assests

Asset là các đối tượng, object được lưu persistent như: file, bảng trong [[Relational Database|RDBMS]], Machine learning model, ...

#### Software defined assets

Software defined asset là dagster object mà được định nghĩa bởi dagster code. 1 software defined assets thường bao gồm các giá trị sau:
- `AssetKey` -> indentify asset
- Set of upstream assets key -> để biết asset hiện tại được sinh ra từ những asset nào.
- __op__ mà sẽ có trách nhiệm tạo ra asset từ những upstream assets

### Ops
Ops là core unit thực hiện computing. Nó tương đương với task trong [[Apache Airflow]].