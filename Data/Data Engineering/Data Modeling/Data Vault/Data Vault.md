---
---
# Abstract

Data vault là 1 pháp data modeling. Nó được phát triển sau [[Dimensional Modeling]] nên nó hạn chế được 1 số nhược điểm của Dimensional Modeling.

# Architecture

Ở đây, chúng ta sẽ nói đến Data Vault 2.0, là phiên bản được sử dụng ở thời điểm hiện tại (([[2023-08-23]]))

- Data vault modeling là 1 thành phần của Data Vault

Các bảng trong Data Vault Modeling được chia ra làm 3 loại chính
- Hub -> Các bussiness __entities__ được quan tâm
- Link -> Liên kết/Link giữa các Hub với nhau.
- Satellites -> Là bảng chứa các thông tin chi tiết, bổ sung cho Hub và Link

## Core concepts

Các thành phần chính của Data vault modeling bao gồm 3 đối tượng
- Hubs
- Links
- Satellites

![[Pasted image 20230823011151.png]]

Ngoài ra, để phù hợp với nhiều yêu cầu đặc thù, một số loại bảng hỗ trợ được thêm vào, cùng với đó là 
### Hubs

> Hubs are entities of interest to the business.

Hubs là các __đối tượng business__ được quan tâm. Các bảng Hubs chỉ chứa Business key của các object để thể hiện là đối tượng này có tồn tại trong hệ thống. Các thông tin metadata về đối tượng được lưu trong bảng satellites.

Hubs có ý nghĩa như là 1 bảng ghi lại sự xuất hiện của đối tượng nào đó trong hệ thống.Vì thế các bảng Hubs sẽ chỉ chứa business unique key của đối tượng (Business Key này có thể là composite fields).

Tuy nhiên, do dữ liệu có thể xuất phát từ nhiều nguồn khác nhau, cho nên các bảng Hub cần có trường Data source để cho biết data này tới từ nguồn nào.
Hub cũng sẽ Surrogate Key như Dimensional Modeling -> phân biệt các đối tượng có thể tới từ các nguồn khác nhau. Giá trị của Surrogate Key này thường sẽ là [[Hash function|hash]] value của: Business Key + Source

Ngoài ra, các bảng Hub có thể có thêm 1 số cột metadata khác như load timestamp, ...

Có thể thấy là cấu trúc của các bảng Hub khá đơn giản, nó chỉ chứa Business Key của hệ thống nguồn và 1 số trường hỗ trợ cho quá trình xử lý dữ liệu
### Links

>  Links connect Hubs and may record a transaction, composition, or other type of relationship between hubs

Links là các relationship giữa các Hub -> many to many join tables. Và đương nhiên nó có thể là quan hệ giữa nhiều bảng với nhau. Nó chính là đại diện cho mối quan hệ giữa các đối tượng (Hub) trong thực tế.

Một bảng Links tối thiểu cần có:
- SUROGATE KEY của các Hubs mà Links là liên kết giữa chúng
- SUROGATE KEY của Links
- Các cột hỗ trợ cho việc xử lý dữ liệu: load_ts, source ...

Tất nhiên là Links cũng sẽ có Surrogate Key và các cột hỗ trợ việc xử lý khác như load timestamp, Record source, ...
#### Hierarchical Link

Là các Links được dùng để diễn tả mối quan hệ của các đối tượng trong cùng 1 Hub. Ví dụ: Links của cùng Hub mô tả quan hệ cha con giữa các địa điểm trong HUB_AREA
#### Same-as Link

Là quan hệ giữa cùng 1 đối tượng. 
Ví dụ: Trong bảng Hub có 2 giá trị SĐT, 1 giá trị sai và 1 giá trị là đã được chuẩn hóa -> Khi đó ta có thể dùng 1 Links để liên kết 2 số ĐT này để cho biết đâu là giá trị chính xác

#### Non-historized Link / Transaction Link

> That is perfectly adapted to the immutable facts, so something that happened in the past can neither be undone nor modified. And since the changes will never happen, they don't need to be stored in the satellites

-> Tức là các Link này chứa data không thể bị update (Log, Transaction, ...). Khi đó dữ liệu sẽ được lưu trực tiếp trong bảng Link, không cần bảng satellite -> Join ít hơn.
#### Exploration Link

### Satellites

> 

Satellite được coi là `context` của các đối tượng khác (Hubs, Links). Nó cung cấp các descritive data cho các đối tượng đó. Ví dụ: _Satellite_ của _Hub_User_ có thể sẽ có các thông tin về Tên , tuổi, địa chỉ, ... của User đó

Các Hubs hoặc Links có thể có nhiều hơn 1 satellite -> Điều này nhằm hướng tới khả năng mở rộng cũng như có thể dùng để tăng hiệu năng của các truy vấn (gần giống với sharding).

Không có điều ngược lại. Các Satellite chỉ có 1 liên kết với các bảng Hubs hay Links. Giữa các Satellite cũng không có direct relationship.
#### Multi-active satellite

Các bảng satellite này chứa nhiều giá trị active active/valid at a time.
Ví dụ: 1 người có thể có nhiều số ĐT -> Satellite chứa Phone Number chứa nhiều giá trị active này

Đối với các bảng này, có  cách model:
	- Row-based -> 
	- Set-based
#### Efective satellite

#### System-driven satellite

### Additional Table

#### Point-in-time table (PIT)
#### Bridge table
#### Reference table

Là các bảng thường được reference nhiều bởi các bảng khác

Ví dụ: Satellite có thể chứa Category Code để reference đến bảng Category mà không cần phải tạo thêm bảng Link giữa Product và Category -> giảm sự phức tạp ko cần thiết

Thường thì các bảng reference sẽ là 1 Hub, và các bảng satellite sẽ có Foreign Key tới bảng này.
## Layer

Thông thường các Data warehouse mà implement Data vault method cũng sẽ chia ra làm 3 layers giống như Dimensional Modeling
# Modeling steps

Có nhiều cách tiếp cận với 


## Rules

This is some rules we should follow when modeling with Data Vault 2.0

![[DV-datasheet-v109-A3.pdf]]

# Data Warehouse Architecture

Below is a proposal for Enterprise Data warehouse Architecture
![[Pasted image 20230906010912.png]]

# Comparation

## Pros
- Insert only architecture: Cái này sẽ phù hợp các hệ thống data warehouse hiện đại, hạn chế các nhược điểm của các hệ thống xử lý song song.
- Agile: Co thể hiểu là khả năng thay đổi nhanh chóng với các yêu cầu mới. Do cách thiết kế là Top Down, cho nên các thay đổi về mạt dữ liệu sẽ ít ảnh hưởng đến thiết kế của Data vault. Thông thường sẽ chỉ là thêm các bảng Satellites, Links. Nếu có phát sinh về business thì cũng chỉ là thêm các bảng Hubs và references.
- Structured, dễ dàng thay đổi (flexibility for refactoring)
- Có các khái niệm tương đồng với các mô hình khác -> dễ làm quen và nắm bắt cách dùng
- Hashed: Hash value là deterministic -> do đó có thể load các dữ liệu song song với nhau mà không cần tuân theo mối quan hệ của dữ liệu. Để rõ ràng hơn, khi ta sử dụng Surrogate Key là sequence, thì ta sẽ phải load dữ liệu theo các thứ tự Hubs -> Links -> Satellite. Phải load Hubs đầu tiên thì ta mới lấy được Key của Hubs sau đó các Bảng Links, Sagtellites sẽ sử dụng Key đó để refernce đến Hubs. Tuy nhiên nếu sử dụng Hash value, các giá trị này sẽ luôn là xác định (deterministic) -> Có thể load data parallel. Ngoài ra, nếu có sự cố hay phải chạy backfill thì cá key xây dựng từ Hash value sẽ được vẫn giữ nguyên giá trị. Việc sử dụng hash value còn làm tăng tốc độ xứ lý (Không cần xử lý concurrency), và các data warehouse hiện đại không còn hỗ trợ sequence nữa.
One huge advantage Data Vault has against other data warehousing architectures is that relationships can be added between Hubs with ease. Data Vault focuses on being agile and implementing what is needed to accomplish the current business goals. If relationships aren’t currently known or data sources aren’t yet accessible, this is ok because Links are easily created when they are needed. Adding a new Link in no way impacts existing Hubs or Satellites
## Cons
- Complex
- Quá nhiều join
- Cần hiểu rõ về business knowledge
# References
1. [How to implement data vault model - Aginic](https://aginic.com/blog/modelling-with-data-vaults/)