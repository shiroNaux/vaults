---
aliases
  - OLAP
  - Online analytical proccessing
---

# Online analytical proccessing

![[../../_images/ICLH_Diagram_Batch_01_09-OLAP-DataCube-WHITEBG.webp]]

# Trait
- At the core of any OLAP system is an OLAP cube (also called a 'multidimensional cube' or a [[../../Hypercube]]. It consists of numeric facts called _measures_ that are categorized by dimensions. The measures are placed at the intersections of the hypercube, which is spanned by the dimensions as a [[../../Vector Space]]. The usual interface to manipulate an OLAP cube is a matrix interface, like Pivot tables in a spreadsheet program, which performs projection operations along the dimensions, such as aggregation or averaging.
- Each _measure_ can be thought of as having a set of _labels_, or meta-data associated with it. A _dimension_ is what describes these _labels_; it provides information about the _measure_.

## Multidimensional databases

- Multidimensional structure is defined as "a variation of the relational model that uses multidimensional structures to organize data and express the relationships between data". The structure is broken into cubes and the cubes are able to store and access data within the confines of each cube. "Each cell within a multidimensional structure contains aggregated data related to elements along each of its dimensions".  Even when data is manipulated it remains easy to access and continues to constitute a compact database format. The data still remains interrelated. Multidimensional structure is quite popular for analytical databases that use online analytical processing (OLAP) applications. Analytical databases use these databases because of their ability to deliver answers to complex business queries swiftly. Data can be viewed from different angles, which gives a broader perspective of a problem unlike other models.

## Aggregations

- It has been claimed that for complex queries OLAP cubes can produce an answer in around 0.1% of the time required for the same query on [[OLTP]] relational data. The most important mechanism in OLAP which allows it to achieve such performance is the use of _aggregations_. Aggregations are built from the fact table by changing the granularity on specific dimensions and aggregating up data along these dimensions, using an _aggregation function_. The number of possible aggregations is determined by every possible combination of dimension granularities.
- Because usually there are many aggregations that can be calculated, often only a predetermined number are fully calculated; the remainder are solved on demand. The problem of deciding which aggregations (views) to calculate is known as the view selection problem. View selection can be constrained by the total size of the selected set of aggregations, the time to update them from changes in the base data, or both. The objective of view selection is typically to minimize the average time to answer OLAP queries, although some studies also minimize the update time. View selection is [[NP-Complete]]. Many approaches to the problem have been explored, including [[greedy algorithms]], randomized search, [[genetic algorithms]] and [[A* search algorithm]].
- Some aggregation functions can be computed for the entire OLAP cube by _**precomputing**_ values for each cell, and then computing the aggregation for a roll-up of cells by aggregating these aggregates, applying a [[divide and conquer algorithm]] to the multidimensional problem to compute them efficiently. For example, the overall sum of a roll-up is just the sum of the sub-sums in each cell. Functions that can be decomposed in this way are called **_decomposable aggregation functions_**, and include `COUNT`, `MAX`, `MIN`, and `SUM`, which can be computed for each cell and then directly aggregated; these are known as self-decomposable aggregation functions. In other cases the aggregate function can be computed by computing auxiliary numbers for cells, aggregating these auxiliary numbers, and finally computing the overall number at the end; examples include `AVERAGE` (tracking sum and count, dividing at the end) and `RANGE` (tracking max and min, subtracting at the end). In other cases the aggregate function cannot be computed without analyzing the entire set at once, though in some cases approximations can be computed; examples include `DISTINCT COUNT, MEDIAN,` and `MODE`; for example, the median of a set is not the median of medians of subsets. These latter are difficult to implement efficiently in OLAP, as they require computing the aggregate function on the base data, either computing them online (slow) or precomputing them for possible rollouts (large space).

# Classify

## 1. MOLAP
## 2. ROLAP
## 3. HOLAP

## Comparation types of OLAP 
- Some MOLAP implementations are prone to database explosion, a phenomenon causing vast amounts of storage space to be used by MOLAP databases when certain common conditions are met: high number of dimensions, pre-calculated results and sparse multidimensional data.
- MOLAP generally delivers better performance due to specialized indexing and storage optimizations. MOLAP also needs less storage space compared to ROLAP because the specialized storage typically includes [[compression]] techniques.
- ROLAP is generally more scalable. However, large volume pre-processing is difficult to implement efficiently so it is frequently skipped. ROLAP query performance can therefore suffer tremendously.
- Since ROLAP relies more on the database to perform calculations, it has more limitations in the specialized functions it can use.
- HOLAP attempts to mix the best of ROLAP and MOLAP. It can generally pre-process swiftly, scale well, and offer good function support.

# Distinction

## 1. [[OLTP]]

 | OLAP                                                                                                                                       | OLTP                                                                                                     |
 | ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
 | OLAP system cho phép truy xuất data cho các phân tích phức tạp, để tạo phân tích, báo cáo, DSS,..                                          | OLTP systems are ideal for making simple updates, insertions and deletions in databases                  |
 | Query thường truy vấn số lượng data lớn                                                                                                    | Query thường tác động đến một số ít các records|
 | An OLAP database has a multi-dimensional schema, so it can support complex queries of multiple data facts from current and historical data | OLTP, on the other hand, uses a traditional DBMS to accommodate a large volume of real-time transactions, Different OLTP databases can be the source of aggregated data for OLAP, and they may be organized as a data warehouse|
 | Thời gian trả về kết quả tương đối lớn, do phải làm việc với số lượng dữ liệu lớn -> nhu cầu tối ưu để ra kết quả trong thời gian sớm nhất | For OLTP transactions and responses, every millisecond counts. Workloads involve simple read and write operations via SQL (structured query language), requiring less time and less storage space|

- Tóm lại: OLAP và [[OLTP]] có mục đích riêng của chúng, ***không thể thay thế lẫn nhau***, và thường được kết hợp với nhau: nguồn dữ liệu của OLAP thường sẽ là từ nhiều nguồn [[OLTP]] kết hợp lại và được biến đổi thông  qua quá trình [[ETL]]

## 2. [[OLEP]]

## 3. [[Data warehouse]]


# References
1. [OLTP Và OLAP Có Gì Khác Nhau? (viblo.asia)](https://viblo.asia/p/oltp-va-olap-co-gi-khac-nhau-maGK786BZj2)
2. [Tổng quan về Xử lý Phân tích Trực tuyến (OLAP) (microsoft.com)](https://support.microsoft.com/vi-vn/office/t%E1%BB%95ng-quan-v%E1%BB%81-x%E1%BB%AD-l%C3%BD-ph%C3%A2n-t%C3%ADch-tr%E1%BB%B1c-tuy%E1%BA%BFn-olap-15d2cdde-f70b-4277-b009-ed732b75fdd6)
3. [OLAP vs. OLTP: What’s the Difference? | IBM](https://www.ibm.com/cloud/blog/olap-vs-oltp)
4. [Online analytical processing - Wikipedia](https://en.wikipedia.org/wiki/Online_analytical_processing)
