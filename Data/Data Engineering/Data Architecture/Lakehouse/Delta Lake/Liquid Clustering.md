# Introduction

Liquid Clustering là 1 cơ chế lưu trữ, sắp xếp các data file trên Hard disk nhằm mục đích tối ưu khả năng truy vấn của [[Delta Lake]] table format.


# Mechanism

![[delta_liquid_clustering.png]]

Các [[Database|database]] thường sử dụng sorting hoặc partition để optimize tốc độ truy vấn. Tuy nhiên khi sử dụng partitioning thì sẽ có nhiều các restrictions, cụ thể là các câu query sẽ phải sử dụng các cột dùng để partition để filter data. Do đó mà mức độ dynamic của các câu truy vấn bị giảm đi.

Ví dụ, nếu 1 bảng đã được partition theo `region` thì các câu truy vấn bắt buộc phải filter theo cột này thì mới tận dụng được việc partitioning. Có thể hạn chế 1 phần bằng cách partition theo nhiều cột. Tuy nhiên nếu số lượng cột quá nhiều và data có mức cardinality quá cao thì việc partition này sẽ không còn nhiều hiệu quả.

Liquid Clustering sử dụng tree-based (**cụ thể là ???**) để optimize data layout.

# References
1. https://docs.databricks.com/aws/en/delta/clustering
2. https://delta.io/blog/liquid-clustering/