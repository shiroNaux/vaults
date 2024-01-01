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
	2. Private Key thường sẽ không được hiển thị đối với người dùng mà sẽ được 1 ứng dụng nào đó quản lý. và để truy cập được Khóa bí mật đó thì người dùng phải thực hiện các thao tá