LVM là 1 công cụ dùng để quản lý các ổ cứng và phân vùng của chúng trên [[Linux]].

# Abstraction

## Physical Volume (PV)

Physical volume chính là physical volume :)))

Nó có thể là Disk hoặc là 1 partition cúa [[Disk]]

## Volume Group 

Volume Group là 1 tập các Physical volume nhóm lại với nhau. Đây chính là logical level đầu tiên. 

![[Pasted image 20230917222647.png]]

Volume Group sẽ hoạt động giống như 1 hard disk ảo. Nguồn vào của VG chính là các partition từ các hard disk thật. Có thể thấy, nêú dùng VG hoàn toàn có thể tạo nên được 1 mount point có dung lượng = tất cả các hard disk cộng lại. LVM sẽ tự tổ chức và quản lí lưu trữ dữ liệu trên VG tương ứng với ở đâu trên các hard disk.
## Logical Volume

Logical volume là các volume được tách ra từ Volume Group. Chúng hoạt động giống với các partition thông thường. Để lưu trữ file system thì cần phải mount vào 1 mount point giống như partition.

Tóm lại LVM sẽ
- Gộp các hard disk lại để tạo ra các VG
- Từ các VG này sẽ tạo ra Logical Volume

So với cơ chế Disk -> Partition truyền thống, LVM có nhiều hơn 1 layer. Điều này giúp việc quản lý các phân vùng hay ổ cứng dễ dàng hơn (🥲)
# Advantages

