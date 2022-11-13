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

## Implement


Ví dụ cụ thể:
![[../../_images/113031253-791e9c00-915c-11eb-8e84-bc743c8cafb8.png]]
Trên đây là 1 ví dụ được đưa ra bởi người/tổ chức đề xuất modeling này (https://github.com/ActivitySchema/ActivitySchema/blob/main/spec.md)
Có 1 số điểm cần lưu ý:
- Các cột: feature_1, feature_2, feature_3 là các cột store metadata của activity. Do activity stream chứa mọi activity khác nhau -> tên cột giống nhau nhưng ý nghĩa là khác nhau cho từng dòng dữ liệu, phụ thuộc vào loại activity.
	- Ví dụ: For example, for the 'received_email' activity, **feature_1** could be the sender email, **feature_2** could be the subject, and **feature_3** could be the marketing campaign used. For the 'viewed_page' activity **feature_1** could be the url, and **feature_2** could be the referrer
- Thông thường, các activity chỉ chứa metadata liên quan đến nó, nếu cần thêm các thông tin khác thì có 2 cách:
	- Borrow features: sef [[join]] activity stream để lấy được những thông tin cần thiết. Đây là cách thường được sử dụng do mọi data đã được lưu trong activity stream.
	- Enrichment table: là 1 loại bảng khác trong activity schema. Có khóa tham chiếu đến activity_id trong activity stream. Ngoài ra còn chứa thêm các cột khác để bổ sung thông tin cho activity.
# Pros
- A table -> single source of truth -> query cho mọi báo cáo trở nên đơn giản hơn. 
- Thời gian develop nhanh. Bao gồm các bước:
	- Các thao tác transform dễ dàng hơn, nhanh hơn
	- Query cho dashboard đơn giản hơn
- Activivty schema có thể được áp dụng cho mọi lĩnh vực, bài toán khác nhau (consistent format) với chung 1 schema, model. Nếu dùng các model khác thì phải thay đổi theo đặc điểm của từng công ty, lĩnh vực. -> automation dễ dàng,reusable
- Tốc độ truy nhanh hơn, query đơn giản hơn.

# Cons
- single source of truth -> single point of failure. 
- Do đặt mọi data trong cùng 1 bảng -> 1 bảng quá lớn, không phù hợp với các công cụ hiện tại như:
	- Hầu hết các database không xử lý tốt với 1 bảng có quá nhiều dữ liệu do vấn đề về [[Allocation]].
	- 1 số tools NI như PowerBI, Tableau cần đưa data lên [[RAM]] để thực hiện xử lý -> memeory không đủ. Có thể giải quyết = tách ra các bảng nhỏ hơn để tạo báo cáo, nhưng như vậy sẽ làm mất đi ý nghĩa của activity schema.
- Rò rỉ dữ liệu -> all done!

> ⚠️Hiện chưa có 1 ví dụ cụ thể, chính xác về activity schema -> không có kết luận chính xác về việc áp dụng activity schema trong thực tế
# References
1. [ActivitySchema/spec.md at main · ActivitySchema/ActivitySchema (github.com)](https://github.com/ActivitySchema/ActivitySchema/blob/main/spec.md)
2. [DWH Modeling: Opinions on Activity Schema? : SQL (reddit.com)](https://www.reddit.com/r/SQL/comments/qj1czv/dwh_modeling_opinions_on_activity_schema/)