---
---
# UEFI
UEFI là viết tắt của **Unified Extensible Output System**, nó là 1 [[firmware]] mà đi kèm theo [[mother board]] -> đi theo hãng sản xuất [[mother board|main]]. Do nó là [[firmware]] gắn theo _main_ -> nó là chương trình được chạy đầu tiên khi khởi động máy tính. UEFI có 3 nhiệm vụ chính
- Kiểm tra các phần cứng ([[Disk]], [[RAM]], [[CPU]], ...) được gắn vào _main_ -> nếu có bất thường sẽ có thông báo lỗi
- Khởi độngd các thành phần vừa kiểm tra
- Chuyển giao các phần đó cho [[OS]] để máy tính hoạt động
=> Quá trình này được gọi là [[Boot]]

Ngoài ra nó còn có khả năng quyết định các thông số phần cứng như: clock [[CPU]], [[RAM]] latency, hay tốc độ của quạt, ... Ngoài ra nó còn được dùng để kiểm tra khả năng hoạt động của các thiết bị phần cứng.

# How UEFI Boot work


# UEFI vs Legacy BIOS

# References

1. https://beebom.com/what-is-uefi/
2. 