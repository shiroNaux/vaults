# GraphQL


## Why GraphQL ?

1. Với cách dùng [[REST]] [[../API]] thông thường có một vài điểm yếu:
	- Với mỗi loại dữ liệu nhất định -> cần 1 endpoint, dẫn đến nếu xây dựng 1 app lớn thì sẽ có quá nhiều endpoint cần maintain -> tăng độ phức tạp
	- Sử dụng REST API sẽ dẫn đến việc fetching quá nhiều do 1 REST API sẽ không cung cấp được đầy đủ thông tin -> tăng số lần request. Ví dụ: Lấy UserID -> lấy FriendsID -> lấy thông tin của Friend -> dẫn đến bài toán [[../../../Data/Database/N+1 Problem]] . Hoặc chỉ cần lấy 1 field nhưng [[REST]] API trả về số field nhiều hơn thế. Những việc này dẫn đến -> tốn rất nhiều tài nguyên tính toán, bandwith, ...
	- Với các dữ liệu có dạng nested -> Xuất hiện waterfall request
	- Các lợi ích khác mà [[GraphQL]] mang lại
		- Strongly-typed schema: Schema của dữ liệu phải được định nghĩa rõ ràng bằng GraphQL Schema Definition Language (SDL) -> Giảm thiểu một số lỗi liên quan đến schema
		- Hỗ trợ sử dụng biến và tham số khiến query dễ dàng hơn.
		- Versionless -> Vừa là điểm mạnh, vừa là điểm yếu

# Abstraction

## Glossaries

- Type: -> struct trong [[C]]/[[C++]] hay object trong 1 số ngôn ngữ lập trình [[Object Oriented Programming]].
- Resolver: Là 1 function. Mục đích của resolver là retrieve data từ nguồn dựa theo câu truy vấn của client gửi đến
- Mutation: Là 1 request dùng để sửa đổi data trên server. Nó giống với ý nghĩa các method: POST, PUT, DELETE trong [[REST]].
- SDL: Schema definition language -> Ngôn ngữ để define graphql schema
- federation: 
## Hạn chế

### GraphQL vs OData vs REST

### References
1. [Why use GraphQL? | Apollo GraphQL Blog](https://www.apollographql.com/blog/why-use-graphql)
