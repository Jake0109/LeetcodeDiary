# Leetcode No.64 Minimum Path Sum

## Description

Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example:**

```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

## My Solution

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        len_row = len(grid)
        len_column = len(grid[0])

        sum_path = [[0]*len_column for _ in range(len_row)]

        for row in range(len_row):
            for column in range(len_column):
                if row == 0 and column == 0:
                    sum_path[0][0] = grid[0][0]
                elif row == 0:
                    sum_path[row][column] = sum_path[row][column-1] + grid[row][column]
                elif column == 0:
                    sum_path[row][column] = sum_path[row-1][column] + grid[row][column]
                else:
                    sum_path[row][column] = min(sum_path[row-1][column], sum_path[row][column-1]) + grid[row][column]

        return sum_path[-1][-1]
```

还是一个DP的思路，跟前两题相似。



## Reflection & Polishing-up

小小优化一下：

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        len_row = len(grid)
        len_column = len(grid[0])

        sum_path = [[0]*len_column for _ in range(len_row)]

        sum_path[0][0] = grid[0][0]

        for i in range(1, len_row):
            sum_path[i][0] = sum_path[i-1][0] + grid[i][0]
        for i in range(1, len_column):
            sum_path[0][i] = sum_path[0][i-1] + grid[0][i]

        for row in range(1, len_row):
            for column in range(1, len_column):
                sum_path[row][column] = min(sum_path[row-1][column], sum_path[row][column-1]) + grid[row][column]

        return sum_path[-1][-1]
```

这样就不用每次循环过程中都去多判断两次。

## Conclusion

去讨论区看了一下，鉴于大家做法都大致相同，所以我提一点自己的思考：

- 如果不是需求，不要直接更改传进来的参数，比如这里的grid。
- 在一个异步系统中谁也不知道哪一个函数先被调用，如果说，这个数组被更改过，然后被另外一个需要原来数组数据的函数调用了，那么就会出问题。而且问题的排查会非常麻烦，因为一个异步系统的结果输出是无法复现的。
- 我个人推荐将结果作为函数的返回值传递。