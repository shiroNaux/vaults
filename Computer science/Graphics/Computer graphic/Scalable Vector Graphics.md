---
tags:
  - SVG
  - svg
aliases:
  - SVG
---
# Introduction

SVG stand for Scalable Vector Graphics

# 

Đoạn code dưới sẽ tạo ra 1 hình tròn
<svg 
  width="100" 
  height="100" 
  viewBox="0 0 200 200"
>
  <circle cx="100" cy="100" r="50" />
</svg>

Các thuộc tính trong thẻ svg
- width và height: -> size của svg
- viewBox -> define hệ tọa độ mà các đối tượng có trong svg sẽ neo vào làm mốc quy chiếu
	- 2 giá trị đầu là tạo độ của góc trên trái
	- 2 giá trị sau là tọa độ của góc dưới phải
- Các giá trị tọa độ có thể không nhất thiết phải = hoặc tương ứng với height và width

Ví dụ:

<svg 
  width="200"
  height="200"
  viewBox="-100 -100 200 200"
>
  <circle 
    cx="0"
    cy="20"
    r="70"
    fill="#D1495B" 
  />
</svg>


# Elements

## Rectangle

## Circle

## Ellipse

# References
1. [Day 1: How to Draw Basic Shapes with SVG - SVG Tutorial (svg-tutorial.com)](https://svg-tutorial.com/svg/basic-shapes)
2. [SVG Rectangle (w3schools.com)](https://www.w3schools.com/graphics/svg_rect.asp)