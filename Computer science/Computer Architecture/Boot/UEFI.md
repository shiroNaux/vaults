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
- Tiếp theo UEFI sẽ scan toàn bộ _bootable storage devices_ được gắn vào máy mà chúng phải sử dụng định dạng [[GPT]] -> Cần có phần cứng và phần mềm phù hợp mới có thể sử dụng UEFI. Tiếp đó nó sẽ scan [[GPT]] để tìm được _EFI Service Partition_ để bắt đầu quá trình [[Boot]], tuy nhiên nếu không tìm được UEFI sẽ quay lại chế độ [[Legacy Bios|Bios]].
- Cuối cùng là chuyển giao phần cứng cho OS
![[UEFI_boot_process.jpg]]
# UEFI vs Legacy BIOS
- UEFI được ra đời vào năm 2005, còn [[Legacy Bios|Bios]] thì là từ thế kỉ trước. UEFI được phát triển nhằm cải thiện các điểm yếu của [[Legacy Bios|Bios]], tuy nhiên nó không thay thế hoàn toàn. Cả 2 đều sử dụng 1 thành phần là: ***BIOS ROMs***. Điểm khác biệt giữa chúng chính là cách mà chúng tìm _bootloader_.
- Bước đầu tiên trong quá trình hoạt động của Bios sau khi được đánh thức cũng là kiểm tra phần cứng. Điểm khác biệt chính là bios sẽ scan bootable với định dạng [[MBR]]
- Vì ra đời sau, nên UEFI có nhiều ưu điểm sơ với [[Legacy Bios]]
	- Storage: Bios hoạt động với 16 bits ~ 1 MB nên dung lượng dành cho các hoạt động là rất ít. Ngoài ra Bios còn đi kèm với [[MBR]] nên dung lượng tối đa là ~ 2.2T. Còn UEFI sử dụng [[GPT]] với 64 bit entries -> dung lượng lớn hơn rất nhiều, theo ước tính lên đến ~ 9.4 zettabytes
	- Tốc độ [[Boot]]
	- Secure [[Boot]]

# Attack on UEFI
Đã có những malware hay trojan nhắm tới UEFI, có thể kể đến như
- Sednit hay còn gọi là APT28, Fancy Bear lợi dụng UEFI để access hard drive
- Trickbot
# References

1. [What Is UEFI and How It Differs from BIOS? | Beebom](https://beebom.com/what-is-uefi/)
2. [UEFI - Wikipedia](https://en.wikipedia.org/wiki/UEFI)
3. [assembly - How does UEFI work? - Stack Overflow](https://stackoverflow.com/questions/32223339/how-does-uefi-work)
4. [Sự khác biệt giữa UEFI và BIOS - QuanTriMang.com](https://quantrimang.com/cong-nghe/su-khac-biet-giua-uefi-va-bios-169895#)