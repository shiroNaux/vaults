LVM là 1 công cụ dùng để quản lý các ổ cứng và phân vùng của chúng trên [[Linux]].

# Abstraction

## Physical Volume (PV)

Physical volume chính là physical volume :)))

Nó có thể là Disk hoặc là 1 partition cúa [[Disk]]

## Volume Group 

Volume Group là 1 tập các Physical volume nhóm lại với nhau. Đây chính là logical level đầu tiên. 

![[Pasted image 20230917222647.png]]

Volume Group sẽ hoạt động giống như 1 hard disk ảo. Nguồn vào của VG chính là các partition từ các hard disk thật. Có thể thấy, nêú dùng VG hoàn toàn có thể tạo nên được 1 mount point có dung lượng = tất cả các hard disk cộng lại. LVM sẽ tự tổ chức và quản lí lưu trữ dữ liệu trên VG tương ứng với ở
## Logical Volume

# Advantages

