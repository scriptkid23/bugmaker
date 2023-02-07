---
title: Giới thiệu về Big Oh
date: '2023-02-07'
tags: ['Algorithms', 'Big O']
draft: false
images:
  [
    '/static/images/Ti%CC%80m%20hie%CC%82%CC%89u%20ve%CC%82%CC%80%20Big%20O%2087156e0cfce74ad9b73381abdf33b4fb/thumbnail.jpg',
  ]
summary: Giới thiệu các khái niệm cơ bản về Big O và cách phân tích một thuật toán.
---

# Khái niệm về Running Time

Bất cứ khi nào chúng ta nói về một thuật toán, thông thường sẽ thảo luận về thời gian chạy của nó (Running time). Mục đích cuối vẫn là lựa chọn thuật toán hiệu quả cho dù bạn đang cố gắng tối ưu hóa theo thời gian hay không gian.

Quay lại với bài toán tìm kiếm. Cách tiếp cận đầu tiên là kiểm tra từng số, từng số một(Simple search). Nếu đây là một mảng gồm 100 số thì cần tới 100 lần đoán. Nếu đó là một mảng gồm 4 tỷ số, thì phải có tới 4 tỷ lần đoán. Vì vậy, số lần đoán tối đa bằng với kích thước của mảng. Đây được gọi là thời gian tuyến tính (linear time).

Nhưng với tìm kiếm nhị phân thì khác. Nếu mảng dài 100 số, sẽ cần tối đa 7 lần đoán. Nếu là 4 tỷ số, chỉ cần tối đa 32 lần đoán. Đây được gọi là thời gian logarit (hoặc log time)

![Untitled](/static/images/Ti%CC%80m%20hie%CC%82%CC%89u%20ve%CC%82%CC%80%20Big%20O%2087156e0cfce74ad9b73381abdf33b4fb/Untitled.png)

Với Binary Search ta có

$log_2100\simeq7$

$log_24000000000\simeq32$

Trong khoa học máy tính, bất cứ khi nào chúng ta nói $O(log n)$, thực ra đó là cách viết tắt của $O (log_2n)$.

# Big O?

Ký hiệu Big O cho bạn biết thuật toán nhanh như thế nào. Nó mô tả chính xác số bước (phép toán) tăng lên như thế nào khi dữ liệu tăng lên.

![Untitled](/static/images/Ti%CC%80m%20hie%CC%82%CC%89u%20ve%CC%82%CC%80%20Big%20O%2087156e0cfce74ad9b73381abdf33b4fb/Untitled%201.png)

Hơn thế nữa, Big O không chỉ quan tâm đến số phép tính mà một thuật toán thực hiện. Nó quan tâm đến quỹ đạo của các bước trong thuật toán khi dữ liệu tăng lên. Ví dụ

![Untitled](/static/images/Ti%CC%80m%20hie%CC%82%CC%89u%20ve%CC%82%CC%80%20Big%20O%2087156e0cfce74ad9b73381abdf33b4fb/Untitled%202.png)

<aside>
💡 Tuy nhiên, khi hai thuật toán thuộc cùng một phân loại của Big O đi chăng nữa thì chưa chắc cả hai thuật toán đều có cùng tốc độ. Ví dụ Bubble Sort chậm gấp đôi so với Selection Sort mặc dù cả hai đều là $O(n^2)$. Big O sẽ nổi trội khi sử dụng để so sánh cho các thuật toán có `Big O` khác nhau nhưng khi hai thuật toán thuộc cùng một `Big O`, thì cần phải phân tích thêm để xác định thuật toán nào nhanh hơn.

</aside>

### Phân tích thuật toán Bubble Sort

Cho một mảng **arr [4,2,7,1,3] sắp xếp theo thứ tự tăng dần**

Bước 1: So sánh số 4 và 2

arr = 4,2,7,1,3

Bước 2: Nhận thấy không đúng thứ tự do vậy đổi chỗ ta được

arr = 2,4,7,1,3

Bước 3: So sánh 4 và 7 nhận thấy đúng thứ tự do vậy không phải đổi chỗ

arr = 2,4,7,1,3

Bước 4: So sánh 7 và 1 nhận thấy không đúng thứ tự

Bước 5: Đổi chỗ

arr = 2,4,1,7,3

Bước 6: So sánh 7 và 3 nhận thấy không đúng thứ tự

Bước 7: Đổi chỗ

arr = 2,4,1,3,7

Do đó bây giờ thay vì ta xử lý 2,4,1,3,7 ta sẽ chỉ cần xử lý mảng 2,4,1,3 vì vị trí của số 7 đã đứng đúng vị trí của nó

Dưới đây là một triển khai thuật toán Bubble Sort trong Python

```python
def bubble_sort(list):
	unsorted_until_index = len(list) - 1
	sorted = False
	while not sorted:
		sorted = True
		for i in range(unsorted_until_index):
			if list[i] > list[i+1]:
				list[i], list[i+1] = list[i+1], list[i]
				sorted = False
		unsorted_until_index -= 1
return list
```

Nhận thấy thuật toán bao gồm 2 bước chủ đạo

- **Comparisons**
- **Swaps**

Với mảng 5 phần tử ta nhận thấy bước **Comparisons** có tất cả

$$
4+3+2+1=10
$$

Do vậy với mảng n phần tử ta có công thức tính số bước của **Comparisons**

$$
(n-1)+(n-2)+...+1
$$

Trong Worst-case, khi mà số lượng bước Swaps bằng với số lượng bước Comparisons thì ta có tổng số bước cần thực hiện là

- 20 steps với mảng 5 phần tử
- 90 steps với mảng 10 phần tử
- 380 steps với mảng 20 phần tử

Ta nhận thấy số phần tử tăng lên thì số bước sẽ tăng lên theo giá trị gần với cấp số nhân hoặc có thể nhận thấy thông qua bảng sau

| N Data Elements | # Of Bubble Sort Steps | $N^2$ |
| --------------- | ---------------------- | ----- |
| 5               | 20                     | 25    |
| 10              | 90                     | 100   |
| 20              | 380                    | 400   |
| 40              | 1560                   | 1600  |
| 80              | 6320                   | 6400  |

<aside>
💡 Hãy nhớ rằng, Big O luôn trả lời câu hỏi mấu chốt: Nếu với tập dữ liệu đầu vào N  thì thuật toán sẽ thực hiện bao nhiêu bước.

</aside>

### Phân tích thuật toán Selection Sort

Cho một mảng **arr [4,2,7,1,3] sắp xếp theo thứ tự tăng dần**

Với thuật toán này, ta sẽ phải so sánh với từng phần tử để tìm giá trị nhỏ nhất trong mảng sau đó ta sẽ đặt chúng vào từng vị trí từ 0 → 4

Gọi `lowestNumberIndex` là nơi lưu trữ giá trị nhỏ nhất

Ta có bảng mô ta các bước

| Bước         | arr (1)   | position (2) (vị trí cần xử lý) | Lowest Value (3) | lowestNumberIndex (4) | Mô tả                                                                                               |
| ------------ | --------- | ------------------------------- | ---------------- | --------------------- | --------------------------------------------------------------------------------------------------- |
| 0 (khởi tạo) | 4,2,7,1,3 | 0                               | 4                | 0                     | Bắt đầu                                                                                             |
| 1            | 4,2,7,1,3 | 0                               | 2                | 1                     | min(Lowest Value bước 0 ,2) = min(4,2)                                                              |
| 2            | 4,2,7,1,3 | 0                               | 2                | 1                     | min(2,7)                                                                                            |
| 3            | 4,2,7,1,3 | 0                               | 1                | 3                     | min(2,1)                                                                                            |
| 4            | 4,2,7,1,3 | 0                               | 1                | 3                     | min(1,3)                                                                                            |
| 5            | 1,2,7,4,3 | 0                               |                  |                       | swap(1,4) vì tìm được giá trị nhỏ nhất là 1                                                         |
| 6            | 1,2,7,4,3 | 1                               | 2                | 1                     | min(2,7)                                                                                            |
| 7            | 1,2,7,4,3 | 1                               | 2                | 1                     | min(2,4)                                                                                            |
| 8            | 1,2,7,4,3 | 1                               | 2                | 1                     | min(2,3) (không thực hiện swap vì vị trí 1 vẫn không thay đổi) tức là position == lowestNumberIndex |
| 9            | 1,2,7,4,3 | 2                               | 4                | 3                     | min(7,4)                                                                                            |
| 10           | 1,2,7,4,3 | 2                               | 3                | 4                     | min(4,3)                                                                                            |
| …            | …         | …                               | …                | …                     | …                                                                                                   |

Sau đây là một triển khai thuật toán Selection Sort trong Javascript

```jsx
function selectionSort(array) {
  for (let i = 0; i < array.length - 1; i++) {
    let lowestNumberIndex = i
    for (let j = i + 1; j < array.length; j++) {
      if (array[j] < array[lowestNumberIndex]) {
        lowestNumberIndex = j
      }
    }
    if (lowestNumberIndex != i) {
      let temp = array[i]
      array[i] = array[lowestNumberIndex]
      array[lowestNumberIndex] = temp
    }
  }
  return array
}
```

Xét Worst-case

Với 5 phần tử ta có 4 + 3 + 2 + 1 = 10 Comparisons + 4 Swaps = 14 steps

Với 10 phần tử ta có 45 Comparisons + 9 Swaps = 54 steps

Ta nhận thấy số phần tử tăng lên thì số bước sẽ tăng lên theo giá trị gần với cấp số nhân hoặc có thể nhận thấy thông qua bảng sau

| N Data Elements | Max # of Steps in Selection Sort | Max # of Steps in Bubble Sort |
| --------------- | -------------------------------- | ----------------------------- |
| 5               | 14                               | 20                            |
| 10              | 54                               | 90                            |
| 20              | 199                              | 380                           |
| 40              | 819                              | 1560                          |
| 80              | 3239                             | 6320                          |

Ta thấy cả 2 thuật toán đều cùng thuộc nhóm Big O là $O(n^2)$ nhưng Steps lại khác nhau do đó ta có thể kết luận: Cùng nhóm Big O nhưng chưa chắc 2 thuật toán đem ra so sánh có tốc độ ngang bằng nhau.

Một số loại Big O mà bạn sẽ gặp rất nhiều và được sắp xếp từ nhanh nhất đến chậm nhất

- $O(logn)$
- $O(n)$
- $O(n *logn)$
- $O(n^2)$
- $O(n!)$

# Big O Notation ignores constants.

Nguyên tắc để xác định dạng Big O đó là không bao giờ chứa các giá trị constants, Nếu thuật toán thực thiện $\frac{N^2}{2}$ steps , ta thấy $\frac{1}{2}$ là một hằng số do đó trong Big O nó sẽ ignores giá trị $\frac{1}{2}$

⇒ Ta có $O(n^2)$

Vậy thì đối với $n^2 + 2n + 1$ bước thì sao?

Nhận thấy 1 là constant ⇒ ignores giá trị 1 ⇒ $O(n^2+2n)$

Ta lại có $n^2 +2n$ = $n(n+2)$ ⇒ ignores giá trị 2 ⇒ $n^2$

Vậy ta kết luận $O(n^2 + 2n + 1)  \equiv O(n^2)$
