---
---
# Abstract
- Là 1 thư viện [[Java]] library dùng để indexing và searching, 
- Sử dụng bằng cách nhúng trực tiếp trong ứng dụng [[Java]]
- [[Elastic Search]] và [[Apache Solr]] cũng được phát triển dựa trên [[Apache Lucece]]


# Architecture
Apache Lucene là 1 công cụ để thực hiện full-text searcg. Tức là kết quả trả về bao gồm những kết quả gần nhất, không nhất thiết phải match 100%.

## Mechanism
### Indexing
- Thành phần quan trọng nhất của Lucene chính là index. Nó là 1 schemaless document [[database]]. Index này được tạo bằng cách sử dụng nội dung của các document
- Index trong lucene là `inverted` index, tức là thay vì mapping pages này chứa keywords nào thì sẽ đánh dấu keywords này có trong những pages nào giống như phần chú giải trong các cuốn sách -> điều này làm khả năng tìm kiếm theo keywords sẽ nhanh hơn rất nhiều -> ***??? keywords đưa vào từ điển để search được lựa chọn như thế nào? 1 từ riêng lẻ hay cụm từ? thuật toán cụ thể ??*** -> được điều khiển bởi __Tokenizers__
- Index này được lưu trữ theo cấu trúc _Skip list data structure_ và được lưu trữ trên disk.

Quá trình tạo ra index được mô tả theo ảnh dưới
![[_images/index-803f30ef0c3b124490d75d9213faf27789788739a4fb3e47cf33a69401ed1f00.jpg]]
- Bước đầu tiên là đưa hết dữ liệu có trong document ban đầu về dạng text, sau đó lần lượt append từng field của document ban đầu vào document mới để từ đó xây dựng index.
- Bước tiếp theo là phân tích document(mới được tạo) và add nó vào index. Lucene sử dụng 1 thành phần được gọi là _Lucene Analyzer_. Nó bào gồm __Tokenizers__ để đưa text thành các `term` và __TokenFilters__ dùng để thêm, sửa hay xóa các `term` trong index.
- Ví dụ về 1 cách hoạt động của __Tokenizers__: 
	- Nó sẽ đưa text về dạng viết thường, bỏ đi các stop words như: `and`, `the`, ...
	- Bổ sung các form khác của từ như: thêm `running` cho nếu có từ `run`
	- Ngoài ra nó có thể xóa các token theo chỉ định của yêu cầu
- Cả __Tokenizers__ và __TokenFilters__ đều có thể custom lại bằng cách viết các implement bằng [[Java]]

### Searching
Việc tìm kiếm trong Lucene được thực hiện thông qua các truy vấn, giống như các [[database]]. Khi thực thi 1 yêu cầu tìm kiếm, Lucene sẽ tìm mọi matching document và sau đó sắp xếp theo _độ liên quan_ hoặc theo các tiêu chí sắp xếp được chỉ định trong yêu cầu tìm kiếm.
_Độ liên quan_ được tính toán bởi 1 thuật toán tính điểm của Lucene. Cách tính điểm này của Lucene phụ thuộc vào 1 số yếu tố

- Term Frequency: số lần xuất hiện của `term` trong document
- Inverse Document Frequency: Nghịch đảo số lần `term` xuất hiện trong toàn bộ các document
- Field’s length

Tuy nhiên, người sử dụng hoàn toàn có thể tác động đến các yếu tố này bằng cách tự tạo custom Java implementation cho thuật toán tính điểm.


# References
1. https://advancedweb.hu/intro-to-lucene/
2. https://stackoverflow.com/questions/2602253/how-does-lucene-index-documents