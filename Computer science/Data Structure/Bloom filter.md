---
---
# Abstract

Bloom filter là 1 cấu trúc dữ liệu xác suất. Bài toán chính mà bloom filter giải quyết đó là: 
Cho tập S
$$
\frac{\partial f}{\partial t}
$$
xác định 1 element có thuộc 1 set hay không? Do là cấu trúc dữ liệu xác suất, cho nên kết quả khi sử dụng cấu trúc dữ liệu này có thể cho ra kết quả sai. Tuy nhiên, nó đảm bảo rằng chỉ xảy ra false positive chứ không bao giờ xảy ra false negative. Tức là, kết quả trả về sẽ:
- Không xuất hiện những lỗi dạng: element không thuộc set mà cho thuộc set
- Có thể xuất hiện lỗi dạng: Thuộc set nhưng không được tính
=> những cái thuộc set nhưng ko tính đi đâu, nếu nó thuộc 1 set khác ->  false negative

## Applied

Bloom được sử dụng rất nhiều trong các sản phẩm software, có thể kể dến như:
- Google BigTable, Apache HBase, Apache Cassandra, RocksDb (nói chung là rất nhiều DB NoSQL khác nhau) và PostgreSQL sử dụng Bloom Filter để giảm chi phí tìm kiếm dữ liệu.
- Google Chrome ứng dụng để lọc ra và cảnh báo những trang web gây hại cho người dùng
- [Medium](https://blog.medium.com/what-are-bloom-filters-1ec2a50c68ff) dùng để tránh gợi ý lại những bài viết mà user đã đọc.
- [Chống DDoS](https://eudl.eu/pdf/10.4108/eai.19-6-2018.155865) hệ thống
- Kiểm tra weak password hoặc username/email đã tồn tại trong hệ thống
- Kiểm tra chính tả bằng cách kiểm tra các từ tồn tại trong từ điển

# Refenreces
1. [Bloom Filter - BlogDogy](https://dogy.io/2020/10/06/bloom-filter/)
2. [Bloom filter – Wikipedia](https://en.wikipedia.org/wiki/Bloom_filter)
3. [Tìm hiểu bộ lọc Bloom (Bloom filter) và một số ứng dụng dưới con mắt đời thường – ManhDV's blog (manhdaovan.github.io)](https://manhdaovan.github.io/programming/vi/2019/12/21/tim-hieu-bloom-filter-va-mot-so-ung-dung/)
4. [Preventing DDoS using Bloom Filter: A Survey (eudl.eu)](https://eudl.eu/pdf/10.4108/eai.19-6-2018.155865)
5. [Bloom Filters: Cấu trúc lưu trữ dữ liệu dựa trên xác suất. - Viblo](https://viblo.asia/p/bloom-filters-tai-sao-cac-mang-blockchain-lai-thuong-su-dung-no-GrLZD07eZk0)