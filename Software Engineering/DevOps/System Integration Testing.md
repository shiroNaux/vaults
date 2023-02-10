---
---
System Testing

> Server testing is a process to ensure that all services are stable, a server is secure, and it can withstand high load. As a rule, a testing suite includes a series of test cases to demonstrate the high-level performance and speed.

- System testing thường được thực hiện sau khi hệ thống đã được tích hợp đầy đủ các thành phần và chuẩn bị đưa vào hoạt động
- Sau quá trình kiểm thử, cần có 1 quá trình cải thiện hiệu năng dựa vào những kết quả thu được từ quá trình testing

1. Phân loại
	- ***Performance testing***: Kiểm thử hiệu năng của hệ thống. Bao gồm các testing khác:
		- ***Load testing***: Kiểm thử với workload tăng dần theo mỗi lần test (trong cùng 1 thời điểm). Đánh giá giới hạn phục vụ yêu cầu của hệ thống (tại 1 thời điểm) với lượng tài nguyên hạn định. Hệ thống sẽ được giám sát trong suốt quá trình chạy để tìm ra được điều kiện làm việc lý tưởng của hệ thống, bao gồm:
			- Tracking the time taken for different operations with low-to-high intensity loads on a system (does the server have enough power to withstand the load)
			- Finding out the number of users that can use the product simultaneously
			- Searching for the peak point of the system load while the system still works correctly (after that, your tech team will find out the maximum load for the system)
			- Testing the system performance using stress loads and finding out the load limits for a system
		- ***Stress testing***: Đo đạc khả năng làm việc của hệ thống nếu workload vượt ra ngoài điều kiện là việc bình thường. Mục đích là để kiểm tra sự ổn định của hệ thống dưới điều kiện tải cao. Ngoài ra còn dùng để đánh giá khả năng phục hồi của hệ thống nếu xảy ra sự cố.
			- ***Spike test***: 1 loại ***Stress testing***, với workload được tăng lên 1 cách đột ngột
		- ***Scalability testing***: Khá giống với ***Load testing***, tăng dần workload theo thời gian (***Load testing*** là khẳ năng phục vụ trong cùng 1 thời điểm), dùng để đánh giá khả năng mở rộng của hệ thống. Có thể thực hiện ***Scalability testing*** bằng cách giữ nguyên workload, giảm số lượng tài nguyên ([[RAM]], [[CPU]]) cung cấp theo thời gian.
		- ***Volume testing*** (**Flood testing**): Đánh giá hiệu quả của hệ thống trong trường hợp phải làm việc với một khối lượng lớn dữ liệu
		- ***Endurance testing***: Kiểm tra độ bền, đánh giá khả năng làm việc của hệ thống trong một khoảng thời gian kéo dài. Để tìm kiếm những vấn đề của hệ thống, như *memory leak*
	- ***Penetration testing***: Kiểm thử xâm nhập. Nhằm tìm kiếm những lỗ hổng trong hệ thống mà người dùng có thể khai thác và tấn công. Kiểm thử này thường được thực hiện bởi những người có kĩ năng về bảo mật

2. Mục đích:
	1. Đánh giá khả năng chịu tải của hệ thống
	2. Tìm ra khoảng làm việc phù hợp với tài nguyên của hệ thống
	3. Phát hiện các lỗi tiềm ẩn, chưa xuất hiện trong các quá trình kiểm thử khác
	4. Cải thiện hiệu suất hệ thống

3. Các thông số giám sát
	- Response time: Tổng thời gian từ lúc gửi request và hoàn thành mọi response, xử lý và hiển thị ra cho người dùng
	- Waite time: Khoảng thời gian chờ giữa lúc gửi thành công request và lúc nhận được response đầu tiên
	- Average load time: Thời gian trung bình để từ lúc hoàn thành action đến lúc gửi request thành công
	- Peak response time
	- Error rate
	- Concurrent users
	- Requests per second
	- Transactions passed/failed
	- Throughput
	- [[CPU]] utilization
	- [[RAM|Memory]] utilization
	- [[Disk]] usage
	- Network utilization


# References
1. [What is Server Testing? An Introduction To Server Testing | Orangesoft](https://orangesoft.co/blog/introduction-to-server-testing
2. [Performance Testing Types, Steps, Best Practices, and Metrics (stackify.com)](https://stackify.com/ultimate-guide-performance-testing-and-software-testing/)
3. 