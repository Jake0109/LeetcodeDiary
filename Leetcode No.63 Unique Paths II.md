# Leetcode No.63 Unique Paths II

## Description

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

**Note:** *m* and *n* will be at most 100.

**Example 1:**

```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

## My Solution

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        mem = obstacleGrid.copy()

        for line in mem:
            for row in range(len(line)):
                if line[row] == 1:
                    line[row] = 0
                else:
                    line[row] = 1

        def generate_approach_of_index(row, column):
            if row == 0:
                mem[row][column] = mem[row][column - 1]
            elif column == 0:
                mem[row][column] = mem[row - 1][column]
            else:
                mem[row][column] = mem[row][column - 1] + mem[row - 1][column]

        len_row = len(mem)
        len_column = len(mem[0])
        for row in range(len_row):
            for column in range(len_column):
                if row == 0 and column == 0:
                    continue
                if mem[row][column]:
                    generate_approach_of_index(row, column)

        return mem[-1][-1]
```

DP的思路，跟昨天的题目没什么太大区别。这题就只能用DP来做了，数学法行不通。需要的话，请翻阅No.62的解题笔记。

## Reflection & Polishing-up

我在discussion看到了非常熟悉的一个词条：backtrack，于是我准备自己也试试：

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        self.path = 0

        def find_path(row, column):
            if row == len(obstacleGrid) - 1 and column == len(obstacleGrid[0]) - 1:
                self.path += 1
                return

            if row >= len(obstacleGrid) or column >= len(obstacleGrid[0]) or obstacleGrid[row][column] == 1:
                return
            find_path(row+1,column)
            find_path(row,column+1)

        find_path(0,0)
        return self.path
```

其实我刚准备说这个题目，leetcode没准备什么太过于刁难我们的数据集啊，但是说完就来了一个`[[1]]`那么问题来了，这个起点即终点，终点不可达这种情况应该算是什么呢？薛定谔的终点233，不改了，这种思考太哲学了，真要想，我能想一个下午。

## Conclusion

- 我看到了很好的一个点：一个能用回溯算法的我第一时间没有用回溯。当然可能是因为昨天的题目我就是用的DP，所以今天就用了，如果直接把这个unique paths II拍在脸上，可能我还会用回溯。但是个人认为这个是一个好现象。
- 看样子这次leetcode没有准备像是`[[]]`这样的数据集，谢天谢地（笑