---
tags:
  - passkey
---
# Introduction

Passkey là 1 cách thức mới để chúng ta có thể xác thực danh tính (Login) trên môi trường Internet

Cách thức phổ biến thường dùng hiện tại là sử dụng mật khẩu, còn có thể kết hợp với các mã OTP để nâng cao tính bảo mật.

Passkey là 1 phương pháp mới, ra đời nhằm mục đích loại bỏ các nguy cơ bảo mật đối với phương pháp sử dụng mật khẩu truyền thống. 

# Technical Detail

Passkey sử dụng kỹ thuật mã hóa công khai ([[Public Key Cryptography]]). Về mặt lý thuyết, chúng ta vẫn cần lưu trữ 1 mã bí mật để trao đổi với server (giống với password), tuy nhiên có 1 số điểm khác

1. Khi sử dụng passkey, có 2 khóa được lưu trữ
	1. Public Key sẽ được lưu trên server và Private Key được lưu tại máy cá nhân của người dùng
	2. Private Key thường sẽ không được hiển thị đối với người dùng mà sẽ được 1 ứng dụng nào đó quản lý. và để truy cập được Khóa bí mật đó thì người dùng phải thực hiện các thao tác xác định danh tính



# Benefits of Passkey

Hiện nay thì tất cả các nội dung trên Internet đều đánh giá Passkey cao hơn về mọi mặt. Tuy nhiên việc adopt passkey vẫn cần 1 thời gian dài nữa do nó mới được các công ty lớn phát triển và sử dụng

Các ưu điểm của passkey là
1. Public key được lưu ở server -> Các cuộc tấn công hay lộ dữ liệu phía server gần như không có tác dụng để tìm được thông tin để đăng nhập
2. Private Key được lưu ở máy người đùng và được quản lý bỏi ứng dụng ->
	1. Giảm thời gian cần thiết để login: Nếu sử dụng password + OTP thì rất tố thời gian, còn nếu sử dụng passkey thì chỉ cần để OS quét gương mặt hay vân tay, phần còn lại do các ứng dụng giao tiếp với nhau.
	2. 
# References
1. [TeamPassword | Passkey vs. Password: Which Is Right for You?](https://teampassword.com/blog/passkey-vs-password)