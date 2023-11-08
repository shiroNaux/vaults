---
tags:
  - QR
---
# Abstract

QR là viết tắt của quick response -> mục đích đầu tiên để nó được tạo ra là để track parts across the manufacturing process.

Nó đồng thời được dùng để khắc phục 1 số điểm hạn chế của barcode như:
- Barcode chỉ có thể đọc được từ 1 hướng, QR code có thể được đọc từ mọi angle
- QR có thể lưu nhiều thông tin hơn do nó là dạng ma trận 2 chiều còn barcode là dạng 1 chiều (tối đa 20 kí tự)
# Technical detail

Các mã QR thường có định dạng: các ô hình vuông màu đen được xắp sếp trên nền trắng

## QR Code Version

Có đến 40 version khác nhau của QR code. Mỗi version khác nhau chủ yếu về kích thước của QR code. Version 40 có kích size là 177 \* 177

![[Pasted image 20231108182153.png]]

Mỗi version đều khác nhau về các yếu tố:
- maximum data capacity
- error corection level
- character type

!! Image ->



QR code được chia làm nhiều loại với các kích thước khác nhau. Tuy nhiên mọi QR code đều bao gồm các thành phần sau:
1. Quiet Zone
2. Separator
3. Finder
4. Aligment marking
5. Timing
6. Version Information
7. Format Information
8. Content
![[Pasted image 20231108014847.png]]

## Finder

Điểm nổi bật nhất trong các mã QR chính là 3 hình vuông có kích thước lớn nằm ở 3 góc của mã QR

https://brooker.co.za/blog/2023/10/18/optimism.html

# References

1. [QR codes | Dan Hollick (typefully.com)](https://typefully.com/DanHollick/qr-codes-T7tLlNi)