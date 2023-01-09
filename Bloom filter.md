---
---
# Abstract

Bloom filter là 1 cấu trúc dữ liệu xác suất. Bài toán chính mà bloom filter giải quyết đó là: xác định 1 element có thuộc 1 set hay không?. Do là cấu trúc dữ liệu xác suất, cho nên kết quả khi sử dụng cấu trúc dữ liệu này có thể cho ra kết quả sai. Tuy nhiên, nó đảm bảo rằng chỉ xảy ra false positive chứ không bao giờ xảy ra false negative. Tức là, kết quả trả về sẽ:
- Không xuất hiện những lỗi dạng: element không thuộc set mà cho thuộc set
- Có thể xuất hiện lỗi dạng: Thuộc set nhưng không được tính
=> những cái thuộc set nhưng ko tính đi đâu, nếu nó thuộc 1 set khác ->  false negative