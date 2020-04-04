# Leetcode No.73 Set Matrix Zeroes

## Description

Given a *m* x *n* matrix, if an element is 0, set its entire row and column to 0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**Example 2:**

```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

**Follow up:**

- A straight forward solution using O(*m**n*) space is probably a bad idea.
- A simple improvement uses O(*m* + *n*) space, but still not the best solution.
- Could you devise a constant space solution?

## My Solution

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """

        row_set = set()
        column_set = set()

        height = len(matrix)
        length = len(matrix[0])

        for i in range(height):
            for j in range(length):
                if matrix[i][j] == 0:
                    row_set.add(i)
                    column_set.add(j)

        for row in row_set:
            matrix[row] = [0 for _ in range(length)]

        for column in column_set:
            for i in range(height):
                matrix[i][column] = 0

        return
```

O(m+n) space solution

O(c)额外空间复杂度的还真没想出来，因为暴力解法的话，判断是否是个被置为0的0还是原生的0是个问题。一个方法就是用别的数来表示需要被置零的位置，比如一个负数。

## Reflection & Polishing-up

在官方给的题解中给出了一个优化过的O(c)额外空间复杂度的方案，自己用python实现了一下：

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """

        height = len(matrix)
        length = len(matrix[0])

        is_col = 0
        is_row = 0

        for row in range(height):
            for column in range(length):
                if matrix[row][column] == 0:
                    if row == 0:
                        is_row = 1
                    if column == 0:
                        is_col = 1
                    matrix[row][0] = 0
                    matrix[0][column] = 0

        for i in range(1,height):
            for j in range(1,length):
                if not matrix[i][0] or not matrix[0][j]:
                    matrix[i][j] = 0

        if is_row:
            matrix[0] = [0 for _ in range(length)]

        if is_col:
            for i in range(height):
                matrix[i][0] = 0
```

还是花了不少时间在做的。

思路挺好理解，就是第一遍遍历的时候，如果有0，就将改行的第一个元素和该列的第一个元素置零，作为标记，以后根据标记来对列表更新。也就是这一步的操作：

```python
if matrix[row][column] == 0:
    matrix[row][0] = 0
    matrix[0][column] = 0
```

其中一些标记包括一些变量请自行理解。

## Conclusion

- 其实从时间角度考虑，我个人想出来的办法已经是最优解了，但是还是需要逼自己深入研究。
- 比如我在Reflection模块中写的那个方法，虽然时间上稍逊于使用内存的，但是额外的空间消耗大大减少成为了常数级的，这个在一个大体量的系统中将会是非常有用的。
- 还是老生常谈的那句话，stay hungry, stay foolish.如是而已。