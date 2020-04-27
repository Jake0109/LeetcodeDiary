# Leetcode  No.85 Maximal Rectangle

## Description

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

**Example:**

```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

## My Solution

很难...一时半会没想出来解决方案，去看了看Solution，看看自己能不能实现吧：

```python
    def maximalRectangle(self, matrix):
        if not matrix:
            return 0

        maxArea = 0
        dp = [[0] * len(matrix[0]) for _ in range(len(matrix))]

        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == "1":
                    dp[i][j] = dp[i][j-1] + 1
```

基于DP的解决方案，但是目前只是完成了基础建设工作。

```python
    def maximalRectangle(self, matrix):
        if not matrix:
            return 0

        maxArea = 0
        dp = [[0] * len(matrix[0]) for _ in range(len(matrix))]

        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == "1":
                    dp[i][j] = dp[i][j-1] + 1
        
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if dp[i][j] == 0:
                    continue
                
                width = dp[i][j]
                for k in range(i,-1,-1):
                    width = min(width, dp[k][j])
                    maxArea = max[maxArea, (i-k+1)*width]
        
        return maxArea
```

需要遍历一遍原矩阵，还需要遍历一遍dp矩阵，那实际上是可以只遍历一遍原矩阵就能完成的，那么可以稍微改变一下结构。

是一个O(M*N^2)的算法，所以不能通过一些很大的数据。再快的话，就需要用到栈，类似No.84的算法来完成。

## Reflection & Polishing -up

官方的栈解法是这样的：

```python
class Solution:

    # Get the maximum area in a histogram given its heights
    def leetcode84(self, heights):
        stack = [-1]

        maxarea = 0
        for i in range(len(heights)):

            while stack[-1] != -1 and heights[stack[-1]] >= heights[i]:
                maxarea = max(maxarea, heights[stack.pop()] * (i - stack[-1] - 1))
            stack.append(i)

        while stack[-1] != -1:
            maxarea = max(maxarea, heights[stack.pop()] * (len(heights) - stack[-1] - 1))
        return maxarea


    def maximalRectangle(self, matrix: List[List[str]]) -> int:

        if not matrix: return 0

        maxarea = 0
        dp = [0] * len(matrix[0])
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):

                # update the state of this row's histogram using the last row's histogram
                # by keeping track of the number of consecutive ones

                dp[j] = dp[j] + 1 if matrix[i][j] == '1' else 0

            # update maxarea with the maximum area from this row's histogram
            maxarea = max(maxarea, self.leetcode84(dp))
        return maxarea
```

这个算法，确实...有点厉害的。也不怪这道题通过的数量这么少...

## Conclusion

- 这几天，返校，隔离，确实心情不太好，没有更新，但是这些都不是理由。
- 最近也在调整自己的心理状态，争取这周恢复日更的状态。