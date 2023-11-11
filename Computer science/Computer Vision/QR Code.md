---
tags:
  - QR
---
# Abstract

QR là viết tắt của quick response -> mục đích đầu tiên để nó được tạo ra là để track parts across the manufacturing process.

Nó đồng thời được dùng để khắc phục 1 số điểm hạn chế của barcode như:
- Barcode chỉ có thể đọc được từ 1 hướng, QR code có thể được đọc từ mọi angle
- QR có thể lưu nhiều thông tin hơn do nó là dạng ma trận 2 chiều còn barcode là dạng 1 chiều (tối đa 20 kí tự)

## Error Corection Level
QR codes contain error correction data. The standard offers the following levels of error correction:
- L: (low) about 7% of codewords can be restored
- M: (medium) 15%
- Q: (quality) 25%
- H: (high) 30% (not available for Micro QR codes)

For Micro QR code symbols, the available error correction levels depend on the version:

- M1 has only error detection
- M2 and M3 support L and M levels
- M4 supports L, M and Q levels

# Technical detail

Các mã QR thường có định dạng: các ô hình vuông màu đen được xắp sếp trên nền trắng

> A QR code is a square array of dark and light dots. One dot (or "module") represents one bit of information.

## QR Code Version

Có đến 40 version khác nhau của QR code. Mỗi version khác nhau chủ yếu về kích thước của QR code. Version 40 có kích size là 177 \* 177

![[Pasted image 20231108182153.png]]

Mỗi version đều khác nhau về các yếu tố:
- maximum data capacity
- error corection level
- character type

!! Image ->



QR code được chia làm nhiều loại với các kích thước khác nhau. Tuy nhiên mọi QR code đều bao gồm các thành phần sau:

1. **Quiet Zone**
2. **Separator**
3. **Finder**
4. **Aligment marking**
5. **Timing**
6. **Version Information**
7. **Format Information**
8. **Content**

![[Pasted image 20231108014847.png]]

## Finder

Điểm nổi bật nhất trong các mã QR chính là 3 hình vuông có kích thước lớn nằm ở 3 góc của mã QR

![[Pasted image 20231109011243.png]]

Đây là các vị trí để dánh dấu hướng đọc của mã QR -> nó giúp các ứng dụng có thể dọc mã QR ở bất kỳ angle nào. Hình vuông nhỏ bên trong gọi là **Inner eye**, hình vuông bên ngoài gọi là **Outer eye**.

## Timing pattern

Timing parttern giúp các ứng dụng biết được độ lớn - kích thước của mã QR -> tức là chỉ ra được mã QR này là version bao nhiêu.

Tiiming parttern được detect dựa vào finder như hình dưới

![[Pasted image 20231110011326.png]]

Chúng gồm 2 dải các ô trắng đen liên tiếp, nối giữa các Finder gần nhau. 

## Format info

Format Info cũng được lưu ở gần các finder. Chúng sẽ chứa các thông tin

![[Pasted image 20231110011737.png]]

## QR Code scanning process

1. Point your phone at a QR code.
2. The QR code scanner in your phone’s camera recognizes the three position markers in the QR code. With a sufficient quiet area, your scanner is now aware of where the edges of the QR code are.
3. The scanner begins at the bottom right, where it encounters the _mode indicator._ These four data modules indicate what data type (numeric, alphanumeric, byte, or kanji) the rest of the encoded data is.
4. Next, the scanner encounters the _character count indicator,_ which are the next 8 data modules up from the mode indicator. These indicate how many characters the total encoded data is
5. Knowing the data type and character length, the scanner then continues its zig-zag path along the data modules until all it retrieves all the encoded information and reaches the _end indicator_
6. After reading the final character, the scanner proceeds along its path to the error correction data modules. Within these encoded modules are one of four levels of error correction. Or how much of the QR code’s encoded data is backed up in case of code damage

![[Pasted image 20231109012554.png]]

# Type of QR Code

## QR Code Model 1
## QR Code Model 2

## Micro QR Code

## Snall QR Code

## iQR

iQR QR Code được coi như là 1 phiên bản mở rộng, nâng cấp của mã QR tiêu chuẩn. Tuy nhiên loại mã này được độc quyền sử dụng bởi Denso Wave (công ty sáng tạo ra QR Code). iQR có nhiều ưu điểm hơn so với các mã QR tiêu chuẩn thông thường như:
- Có thể store được nhiều data hơn so với QR thường
- iQR có thể được biểu diễn dưới dạng hình chữ nhật chứ không bắt buộc là hình vuông
- 


## SQRC

## Frame QR

Loại mã QR này sẽ cho phép chèn hình ảnh hoặc các dạng đồ họa khác vào chính giữa của mã QR. Vùng đồ họa này trên mã QR được gọi là QR Code frame

# References

1. [QR codes | Dan Hollick (typefully.com)](https://typefully.com/DanHollick/qr-codes-T7tLlNi)
2. [QR code - Wikipedia](https://en.wikipedia.org/wiki/QR_code)
3. [QR Code Types: Different Types of QR Codes (sproutqr.com)](https://www.sproutqr.com/blog/qr-code-types)