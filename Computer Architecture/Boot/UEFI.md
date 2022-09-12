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

- Khi UEFI bắt đầu thực thi chức năng của mình(được đánh thức), điều đầu tiên chúng làm là kiểm tra power-on self test(POST). Có nghĩa là chúng sẽ kiểm tra các phần cứng được gắn liền theo máy, để đảm bảo được các phần cứng này có thể hoạt động bình thường. 
- Tiếp theo UEFI sẽ scan toàn bộ _bootable storage devices_ được gắn vào máy mà chúng phải sử dụng định dạng [[GPT]]. Tiếp đó nó sẽ scan [[GPT]] để tìm được _EFI Service Partition_
![[../../_images/UEFI_boot_process.jpg]]
- 
# UEFI vs Legacy BIOS
- UEFI được ra đời vào năm 2005, còn [[Legacy Bios|Bios]] thì là từ thế kỉ trước. UEFI được phát triển nhằm cải thiện các điểm yếu của [[Legacy Bios|Bios]], tuy nhiên nó không thay thế hoàn toàn. Cả 2 đều sử dụng 1 thành phần là: ***BIOS ROMs***. Điểm khác biệt giữa chúng chính là cách mà chúng tìm _bootloader_.
- Bước đầu tiên trong quá trình hoạt động của Bios sau khi được đánh thức cũng là kiểm tra phần cứng. Điểm khác biệt chính là bios sẽ scan bootable với định dạng [[MBR]]
# References

1. https://beebom.com/what-is-uefi/
2. 