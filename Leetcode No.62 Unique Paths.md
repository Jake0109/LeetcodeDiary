# Leetcode No.62 Unique Paths

## Description

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
Above is a 7 x 3 grid. How many possible unique paths are there?

 

**Example 1:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

**Example 2:**

```
Input: m = 7, n = 3
Output: 28
```

 

**Constraints:**

- `1 <= m, n <= 100`
- It's guaranteed that the answer will be less than or equal to `2 * 10 ^ 9`.

## My Solution

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        graph = [[0] * n for _ in range(m)]

        for i in range(n):
            graph[0][i] = 1
        for i in range(m):
            graph[i][0] = 1

        for i in range(1, m):
            for j in range(1, n):
                graph[i][j] = graph[i - 1][j] + graph[i][j - 1]
                
        return graph[-1][-1]
```

我这个是DP的解法。因为之前在实习的时候，无聊写过类似的东西，所以很快就做出来了。

## Reflection & Polishing-up

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        graph = [[1] * n for _ in range(m)]

        for i in range(1, m):
            for j in range(1, n):
                graph[i][j] = graph[i - 1][j] + graph[i][j - 1]
                
        return graph[-1][-1]
```

可以这么改来优化一下。另外一个类似的思路是递归思路，dp相当于是加上了缓存的递归，有兴趣可以自己去看看。

接着就是一个不通的思路：数学思路。

在这个思路里面就是纯数学思想来解答这个问题了：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        def fact(a):
            r = 1
            for i in range(2,a+1):
                r *= i
            return r
        
        right = m-1     # the number of right move to reach the goal
        down = n-1      # the number of down move to reach the goal
        
        return fact(right+down)//fact(right)//fact(down)
```

算法是这样，但是理解其中的思路比写出来更加重要：

```
对于一个m * n 的矩阵，假设m行n列，从起点到终点，总共需要向下(m-1)次向右(n-1)次。
每一次向下或是向右是单独的一次操作。
到达终点的可能性就是(m-1)次向下(n-1)次向右的全排列。
res = (m+n-2)!/(m-1)!/(n-1)!
```

其实可以再实验一下二者的性能差异：

写了无数遍的时间装饰器，这里就不再提了，要看的话，到之前的去翻。

我自己又添加了一个方案：

```python
    @time_count
    def uniquePathsMathCached(self, m: int, n: int) -> int:
        mem = [0,1,]
        def fact(a):
            if a < len(mem):
                return mem[a]
            else:
                return a * fact(a-1)

        right = m - 1  # the number of right move to reach the goal
        down = n - 1  # the number of down move to reach the goal

        return fact(right + down) // fact(right) // fact(down)
```

是math方法的带cache的版本，那么来看一下结果吧

```
func: uniquePathsDP, use time: 1.359999999999556e-05
func: uniquePathsMath, use time: 9.200000000000874e-06
func: uniquePathsMathCached, use time: 6.199999999997874e-06
```

显而易见是DP的更快。

## Conclusion

- 不积跬步，无以至千里。不积小流，无以成江海。相信积累，相信复利。
- 感觉现在conclusion有点难写了啊...特别碰到一些，不那么难，也不那么有意思的题目...