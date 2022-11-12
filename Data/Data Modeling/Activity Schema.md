---
---

# Abstract
Activy Schema là 1 spec để xây dựng [[data modeling]] phục vụ cho mục đích analytic. Theo như tác giả, mục đích của activity schema là để giải quyết 1 số hạn chế của [[dimensional modeling]]. Các ưu điểm của activity schema bao gồm

- **only one definition for each concept** - mọi thứ được quy về activity và entity
- **single model layer** - no layers of dependencies
- **simple definitions** - no thousand-line SQL queries to model data
- **no relationships in the modeling laye**r - Vì mọi thứ được đưa về 1 concept duy nhất -> không có denpendecies giữa các đối tượng, khái niệm
- **no foreign key joins** all concepts can be joined together without having to identify how they relate in advance
- **analyses can be run anywhere** — analyses can be shared and reused across companies with vastly different data
- **high performance** - reduced joins and fewer columns leverages the power of modern column-oriented data warehouses
- **incremental updates** - no more rebuilding data models on every update

Cách thức implement activity schema rất đơn giản: đưa mọi bảng về 1 bảng time series duy nhất gọi là activity stream. Các dashboard, report sẽ sử dụng trực tiếp activity stream mà không cần các view hay bảng aggregate.

![[../../_images/113028328-21cafc80-9159-11eb-972c-34d3617eb379.png]]
# Concept
## Notion

Activity schema modeling xoay quanh 2 khía niệm chính là ___entity___ và ___activity___

##### Entity

Entities là đối tượng hay thực thể nào đó mà thực hiện hành động (activity).
##### Activity
Activities là các hành dộng được thực hiện bởi entity
##### Metadata
Metadata là các thông tin đi kèm với activity để làm rõ các activity này. Các ví dụ của metadata như:
- timestamp xảy ra activity
- amount của hóa đơn nếu activity là thanh toán.

Có 2 loại bảng trong activity shema
1. activity stream (one per activity schema): là bảng duy nhất, chứa mọi activity
2. entity table (optional - one per activity schema): là bảng chứa các thông tin của entity. Có thể có nhiều bảng entity khác nhau.


# References
1. [ActivitySchema/spec.md at main · ActivitySchema/ActivitySchema (github.com)](https://github.com/ActivitySchema/ActivitySchema/blob/main/spec.md)
2. [DWH Modeling: Opinions on Activity Schema? : SQL (reddit.com)](https://www.reddit.com/r/SQL/comments/qj1czv/dwh_modeling_opinions_on_activity_schema/)