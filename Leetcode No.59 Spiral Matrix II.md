# Leetcode No.59 Spiral Matrix II

## Description

Given a positive integer *n*, generate a square matrix filled with elements from 1 to *n*2 in spiral order.

**Example:**

```
Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

## My Solution

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        res = [[0 for _ in range(n)] for _ in range(n)]

        start_point = 0
        for i in range(n//2):
            for j in range(i, n-i):
                res[i][j] = start_point + 1 + j - i
                res[n-1-i][n-1-j] = start_point + 2 * (n - 2 * (i + 1) + 1) + j + 1 - i
            start_point += 4 * (n - 2 * i) - 4
        if n % 2:
            res[n//2][n//2] = n**2

        for i in range(n//2):
            for j in range(i + 1,n-1-i):
                res[j][n-1-i] = res[j-1][n-1-i] + 1
                res[n-1-j][i] = res[n-j][i] + 1

        return res
```

这个是一个纯数学的解法，我是一个个考究了matrix中每个格子他对应的步数写下来的。应该还有更好的解法，但是我暂时没想到。

## Reflection & Polishing-up

小小的优化：

```python
class Solution:
    def generateMatrix(self, n: int):
        res = [[0 for _ in range(n)] for _ in range(n)]

        start_point = 1
        for i in range(n // 2):
            for j in range(i, n - i):
                res[i][j] = start_point + j - i
                res[n - 1 - i][n - 1 - j] = start_point + 2 * n - 5 * i + j - 2
            start_point += 4 * n - 8 * i - 4
        if n % 2:
            res[n // 2][n // 2] = n ** 2

        for i in range(n // 2):
            for j in range(i + 1, n - 1 - i):
                res[j][n - 1 - i] = res[j - 1][n - 1 - i] + 1
                res[n - 1 - j][i] = res[n - j][i] + 1

        return res
```

找到一些感觉还挺有意思的：

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        
        matrix = [[0 for _ in range(n)] for _ in range(n)]
        left, right, top, bot = -1, n, -1, n
        pos = 'right'
        x , y = 0, 0
        for i in range(1, (n ** 2) + 1):
            matrix[x][y] = i
            
            if pos == 'right':
                if y + 1 == right:
                    x += 1
                    top += 1
                    pos = 'down'
                else:
                    y += 1
            elif pos == 'down':
                if x + 1 == bot:
                    y -= 1
                    right -= 1
                    pos = 'left'
                else:
                    x += 1
            elif pos == 'left':
                if y - 1 == left:
                    x -= 1
                    bot -= 1
                    pos = 'top'
                else:
                    y -= 1
            elif pos == 'top':
                if x - 1 == top:
                    y += 1
                    pos = 'right'
                    left += 1
                else:
                    x -= 1
        
        return matrix
```

感觉跟web开发很像啊，`request.method == "xxx"`怎么怎么样的，学到了，很有意思。然后还有个思路相近的但是更像算法题解：

```python
class Solution(object):
    def generateMatrix(self, n):
        row = col = count = 0
        size = n-1
        A = [[None for _ in range(n)] for _ in range(n)]
        
        for ith_num in range(1, n**2 + 1):
            if A[row][col]:
                row += 1; col += 1;
                count += 1; size -= 1
                
            A[row][col] = ith_num
            
            if row == count and col < size: col += 1
            elif row < size and col == size : row += 1
            elif row == size and col > count : col -= 1
            elif row > count and col == count: row -= 1
                
        return A
```

## Conclusion

- 总感觉自己写一些没有官方solution的题目的时候，解法很新奇。也没什么不好，多看看就好。