---

---
# Why?
- Programming paradigm là cách phân chia các ngôn ngữ lập trình dựa vào các đặc điểm của chúng
- **The term programming paradigm** refers to **a style of programming**
- It does not refer to a specific language, but rather it refers to the way you program

# Paradigm

```
.
+-- Imperative
|	+-- Procedure
|	+-- Object-Oriented 
+-- Declarative
|	+-- Functional
|	+-- Logic
|	+-- Mathematical
|	+-- Reactive
```


![[../_images/Programming_paradigms.svg]]

## [[Machine code]]

- Lowest level of programming paradigms

## Imperative language

- Imperative programming consists of sets of detailed instructions that are given to the computer to execute in a given order
- Được dựa trên [[Von Neumann architecture]]
- It works by changing the program state through assignment statements
- It performs step by step task by changing state.
- It's called "imperative" because as programmers we dictate exactly what the computer has to do, in a very specific way.
- Các ngôn nhữ trong nhóm này: 
	- COBOL (COmmon Business Oriented Language)
	- FORTRAN ()
	- [[C Programming language|C]]
- Ưu điểm
	- Dễ để implement thuật toán
	- Bao gồm nhiều tính năng như: loop, conditions, variable, ...

### 1. Procedural
- Ví dụ:
	- [[C Programming language |C]]
	- Pascal
- Procedural programming cho phép tách 1 đoạn lệnh thành các procudure đơn giản hơn
- **Procudures không phải là functions**. Điểm khác biệt giữa chúng là functions có `return` kết quả, còn procedures thì không. Mục đích của functions là luôn cho ra 1 kết quả duy nhất nếu được feed bởi cùng 1 input. Còn mục đích của procedures chỉ là thực hiện lệnh và tạo ra các sidde effect.

- Advantages
	- Đơn giản, hiệu quả, perform cao
	- Dễ dàng đóng thành các module và tái sử dụng

### 2. Object-Oriented

- [[Object Oriented Programming|OOP]] là loại ngôn ngữ phổ biến nhất
- The core concept of OOP is to separate concerns into entities which are coded as objects. Each entity will group a given set of information (properties) and actions (methods) that can be performed by the entity.
-  The smallest and basic entity is object and all kind of computation is performed on the objects only.
- Với cách lập trình kiểu này, có thể giải quyết hầu hết các bài toán trong thực tế với việc mô phỏng các đối tượng thành các object
- Các ngôn ngữ thuộc loại [[Object Oriented Programming|OOP]]
	- [[C++]]
	- [[Java]]
	- [[C Sharp#]]
- [[JavaScripts]] không thuộc loại OOP mặc dù có 1 số tính chất của [[Object Oriented Programming|OOP]]

- Advantages
	- Reuse of code through Inheritance
	- Flexibility through Polymorphism
	- High security with the use of data hiding (Encapsulation) and Abstraction mechanisms.

## Declarative language

- Declarative programming là cách lập trình mà không cần nêu rõ các cách mà máy tính thực thi. Chỉ cần input và yêu cầu về kết quả.
- Ví dụ:

``` sql
	select x % 2 == 0
```

``` js
const result = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
               .filter(n => n % 2 === 0)
               .map(a => a * 10)
               .reduce((a, b) => a + b);
```

- Các ngôn ngữ thuộc dạng này thường có đặc trưng là các hàm `map`, `flatMap`, `filter`. Ví dụ điển hình là [[JavaScripts]]

### 1. Functional

- Với Functional programming, chương trình được xây dựng bởi chuỗi các hàm
- Các ngôn ngữ:
	- [[JavaScripts]]
	- [[Scala]]
	- [[Haskell]]
	- Lisp
- Functional programming có đôi chút khác biệt với [[Purely Functional Programming]]
- Được phát triển dựa trên [[Lambda Calculus]]
- Functional programming languages are typically less efficient in their use of [[CPU]] and memory than imperative languages
- For programs that handle large [[matrices]] and multidimensional [[../Data/Database/Database |database]], array functional languages were designed with speed optimizations

### 2. Logic

- Thường syntax của các ngôn ngữ này dựa trên các quy tắc logic
- Các ngôn ngữ nổi bật:
	- Prolog
	- Datalog
	- ASP
- Các câu lệnh của các ngôn ngữ dạng này thường được viết dưới dạng mệnh đề:

```
	H :- B1, …, Bn
```

- H kí hiệu của Head, B1 -> Bn gọi là body

### 3. Mathematical

- Mô phỏng các bài toán thực tế dưới dạng các hệ phương trình, bất phương trình trong toán học và giải chúng
- Đây không hẳn là 1 loại [[Programming Language |ngôn ngữ lập trình]] mà là một phương pháp giải quyết các bài toán bởi việc sử [[Programming Language]]

### 4. Reactive

- Cũng giống như Mathematical programming, đây là 1 mô hình lập trình chứ không hẳn là loại [[Programming Language|ngôn ngữ lập trình]]
- Thường được sử dụng trong các bài toán event-driven

# Parallel proccessing

- Parrallel proccessing không được quy định bởi ngôn ngữ mà bởi implementation của ngôn ngữ đó.

# Mixing paradigm

- Các ngôn ngữ có thể được chia vào nhiều 1 loại
- Ví dụ: [[Scala]] vừa là functional programming, vừa là ngôn ngữ [[OOP]]
- Hay [[JS]] là 1 ngôn ngữ lập trình hàm, nhưng hỗ trợ rất nhiều tính năng của [[OOP]]
- Rất nhiều các ngôn ngữ [[OOP]] đã áp dụng các hàm đặc trưng của functional programming như các hàm `map`, `flatMap`, `lambda expression`, *higher-order functions* vào trong syntax: [[Java]] từ Java 8, [[C Sharp|C#]] từ .NetCore, [[C Programming language|C]]/[[C++]] từ C++ 11

# References
1. [Programming Paradigms – Paradigm Examples for Beginners (freecodecamp.org)](https://www.freecodecamp.org/news/an-introduction-to-programming-paradigms/)
2. [Programming paradigm - Wikipedia](https://en.wikipedia.org/wiki/Programming_paradigm)
3. [Introduction of Programming Paradigms - GeeksforGeeks](https://www.geeksforgeeks.org/introduction-of-programming-paradigms/)
4. [What exactly is a programming paradigm? (freecodecamp.org)](https://www.freecodecamp.org/news/what-exactly-is-a-programming-paradigm/)
