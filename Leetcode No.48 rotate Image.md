## Leetcode No.48 rotate Image

## Description

You are given an *n* x *n* 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

**Note:**

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

```
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**Example 2:**

```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

## My Solution

```python
import copy

class Solution:
    def rotate(self, matrix) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        matrix_copy = copy.deepcopy(matrix)

        for i in range(len(matrix)):
            for j in range(len(matrix)):
                matrix[i][j] = matrix_copy[len(matrix)-1 - j][i]
```

或者说，最后一行可以换成`matrix[i][j] = matrix_copy[-j - 1][i]`我这个是完全当作数学题来做了。那就让我来理一下中间的推导过程：

- 给定一个点`[x][y]`，围绕点`[0][0]`进行一次顺时针90°的旋转，它会到达`[-y][x]`。
- 在本题中，旋转中心假设为点`[m][m]`是`[(len(matrix)-1)/2][(len(matrix)-1)/2]`这里暂时先不用管是不是一个整数。
- 那现在再给定一个点`[x][y]`围绕点`[m][m]`后的点的位置在哪呢？我是用向量的思路解的：
  - 设定旋转后的点的位置为`[x1][y1]`。
  - 根据围绕`[0][0]`旋转的公式进行推导
  - x1-m = -(y-m)
  - y1-m = x-m
- 得到公式：
  - x1 = 2m-y
  - y1 = x
  - 即 x1 = len(matrix) -1 -y
  - y1 = x
- 最终将这个公式带入获得结果。

我个人是挺满意的，毕竟，许久不做数学题了。高中时期的解析几何也算个人的强项了233

## Reflection & Polishing-up

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        for i in range(len(matrix)):
            for j in range(i+1, len(matrix[0])):
                matrix[i][j], matrix[j][i]= matrix[j][i], matrix[i][j]
            matrix[i] = matrix[i][::-1]
```

很厉害啊，我是没想到能这样子分步写出来的，这里特别要注意一点`for j in range(i+1,len(matrix))`这里必须从`i+1`开始遍历，否则，当`[1][2]`和`[2][1]`都被遍历到，就会使这个matrix乱掉了。

## Conclusion

- 计算机最重要的思想：抽象，在这题中得到了比较好的锻炼，数学能力不能丢啊哈哈。