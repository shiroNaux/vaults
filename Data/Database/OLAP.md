---
aliases
  - OLAP
  - Online analytical proccessing
---

# Online analytical proccessing

![[../../_images/ICLH_Diagram_Batch_01_09-OLAP-DataCube-WHITEBG.webp]]

# Trait

# Distinction

## 1. [[OLTP]]

 | OLAP                                                                                                                                       | OLTP                                                                                                     |
 | ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
 | OLAP system cho phép truy xuất data cho các phân tích phức tạp, để tạo phân tích, báo cáo, DSS,..                                          | OLTP systems are ideal for making simple updates, insertions and deletions in databases                  |
 | Query thường truy vấn số lượng data lớn                                                                                                    | Query thường tác động đến một số ít các records|
 | An OLAP database has a multi-dimensional schema, so it can support complex queries of multiple data facts from current and historical data | OLTP, on the other hand, uses a traditional DBMS to accommodate a large volume of real-time transactions, Different OLTP databases can be the source of aggregated data for OLAP, and they may be organized as a data warehouse|
 | Thời gian trả về kết quả tương đối lớn, do phải làm việc với số lượng dữ liệu lớn -> nhu cầu tối ưu để ra kết quả trong thời gian sớm nhất | For OLTP transactions and responses, every millisecond counts. Workloads involve simple read and write operations via SQL (structured query language), requiring less time and less storage space|

- Tóm lại OLAP và [[OLTP]] có mục đích riêng của chúng, ***không thể thay thế lẫn nhau***, và thường được kết hợp với nhau: nguồn dữ liệu của OLAP thường sẽ là từ nhiều nguồn [[OLTP]] kết hợp lại và được biến đổi thông  qua quá trình [[ETL]]

## 2. [[OLEP]]

# References
1. [OLTP Và OLAP Có Gì Khác Nhau? (viblo.asia)](https://viblo.asia/p/oltp-va-olap-co-gi-khac-nhau-maGK786BZj2)
2. [Tổng quan về Xử lý Phân tích Trực tuyến (OLAP) (microsoft.com)](https://support.microsoft.com/vi-vn/office/t%E1%BB%95ng-quan-v%E1%BB%81-x%E1%BB%AD-l%C3%BD-ph%C3%A2n-t%C3%ADch-tr%E1%BB%B1c-tuy%E1%BA%BFn-olap-15d2cdde-f70b-4277-b009-ed732b75fdd6)
3. [OLAP vs. OLTP: What’s the Difference? | IBM](https://www.ibm.com/cloud/blog/olap-vs-oltp)

