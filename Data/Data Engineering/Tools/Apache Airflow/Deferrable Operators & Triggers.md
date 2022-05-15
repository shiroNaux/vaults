# Deferrable Operators & Triggers


#### 1. Lý do cần Deferrable Operators và Triggers
- Nếu dùng sensor thông thường thì kể cả khi sensor đó không hoạt động, nó vẫn chiếm slot trong workers slot -> tốn tài nguyên
- Nếu dùng mode <font style="color:red">reschedule</font> thì có thể thả workers slot nhưng không flexible do chỉ có thể set được khoảng thời gian interval
- Smart sensors cũng có cơ chế tương tự Deferrable operators nhưng chỉ hoạt động được với các sensors. Đôi khi các operators cũng cần đợi khi đạt đủ điều kiện mới thực thi.

=>  Cần đến Deferrable Operators & Triggers

#### 2. Các Thuật ngữ
- 