---
aliases:
---
# Stability
Reference: [Stability and Sorting Algorithms (kirupa.com)](https://www.kirupa.com/data_structures_algorithms/stability_sorting_algorithms.htm)
Stability của thuật toán sắp xếp là khả năng giữ nguyên được các thứ tụ của các phần tử có cùng giá trị  sau khi hoàn thành thuật toán sắp xếp
Ví dụ: Ta có 1 dãy cần sắp xếp như sau

![[Pasted image 20231012225535.png]]
Nếu sau khi sử dụng 1 thuật toán nào đó, ta thu được dãy như sau:
![[Pasted image 20231012225609.png]]

thì thuật toán vừa được sử dụng được coi là stability. Bởi vì Alice và Michael giữ nguyên thứ tự. Ngược lại nếu thu được kết quả như sau:

![[Pasted image 20231012225740.png]]

thì thuật toán đó được coi là unstability

Stability của các thuật toán sắp xếp phổ biến:

| **Sorting Algorithm** | **Stability** | 
| ----------------- | --------- |
| Bublle Sort       | Stable    |
| Selection Sort    | Unstable  |
| Insertion Sort    | Stable    |
| Merge Sort        | Stable    |
| Quick Sort        | Unstable  |
| Heap Sort         | Unstable  |
| Counting Sort     | Stable    |
| Radix Sort        | Stable    |
| Tim Sort          | Stable    |

