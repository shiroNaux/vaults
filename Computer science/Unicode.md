---
aliases: 
tags:
  - unicode
---
# Introduction

**<u>Unicode là charset</u>**

=> Tức là nó giống 1 cái bảng chứa mã và ký tự được hiển thị tương dứng với mã đó.

Ví dụ:
- Chữ `A` -> có code là `65`
- Kí tự katana `ツ` -> `12484`
- The Musical Symbol G Clef `𝄞` -> `119070`
- `💩` -> `128169`

Mã code đại diện cho các kí tự này được gọi là code point.
Và ta có:

> **Unicode = character ⟷ code point**

Hiện tại mã Unicode lớn nhất được gán cho 1 kí tự hay biểu tượng nào đó là: `0x10FFFF` -> có khoảng 1.1 -> hiện tại (2023-11-20) unicode có thể bao gồm tối đa 1.1 triệu kí tự. Tuy nhiên số này vẫn chưa được dùng hết.

Mới chỉ có khoảng 170.000 (~15%) là đã được định nghĩa tương ứng với các kí tự cụ thể. ~11% codepoint được reserve cho private use và còn lại ~800.000 codepoint là chưa được gán cho kí tự nào cả.

Hình ảnh minh họa (😀😀😀)

![[Pasted image 20231120010932.png]]

# Unicode dựng sẵn và Unicode tổ hợp

2 cái này là option khi chọn bảng mã của Unikey. Điểm khác nhau của 2 bảng mã này là:
- Unicode tổ hợp: 

# References
1. [The Absolute Minimum Every Software Developer Must Know About Unicode in 2023 (Still No Excuses!) @ tonsky.me](https://tonsky.me/blog/unicode/)
2. [Can you tell me in a few words the difference between Unicode and UTF-8? - Quora](https://www.quora.com/Can-you-tell-me-in-a-few-words-the-difference-between-Unicode-and-UTF-8)