# Introduction

Liquid Clustering là 1 cơ chế lưu trữ, sắp xếp các data file trên Hard disk nhằm mục đích tối ưu khả năng truy vấn của [[Delta Lake]] table format.


# Mechanism

![[delta_liquid_clustering.png]]

Các [[Database|database]] thường sử dụng sorting hoặc pả

Liquid Clustering sử dụng tree-based (**cụ thể là ???**) để optimize data layout.

# References
1. https://docs.databricks.com/aws/en/delta/clustering
2. https://delta.io/blog/liquid-clustering/