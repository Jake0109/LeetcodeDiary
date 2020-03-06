# Leetcode No.51 N Queens

## Description

The *n*-queens puzzle is the problem of placing *n* queens on an *n*×*n* chessboard such that no two queens attack each other.

![img](https://assets.leetcode.com/uploads/2018/10/12/8-queens.png)

Given an integer *n*, return all distinct solutions to the *n*-queens puzzle.

Each solution contains a distinct board configuration of the *n*-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

**Example:**

```
Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
```

## My Solution

啊，当时自己看回溯算法的时候看过，是回溯的典型例题，现在自己做做试试：

```python
class Solution:
    def solveNQueens(self, n: int):

        chess = [['.' for _ in range(n)] for _ in range(n)]
        resList = []

        def is_valid(row, column):
            for i in range(n):
                if chess[i][column] == 'Q':
                    return False
                if chess[row][i] == 'Q':
                    return False

            for i in range(n):
                for j in range(n):
                    if abs(i - row) == abs(j - column) and chess[i][j] == 'Q':
                        return False
            return True

        def trackback(count=0):
            if count == n and chess not in resList:
                resList.append(copy.deepcopy(chess))
                return

            for i in range(n):
                for j in range(n):
                    if is_valid(i, j):
                        chess[i][j] = 'Q'
                        count += 1
                        trackback(count=count)
                        chess[i][j] = '.'
                        count -= 1

        trackback()
        num = len(resList)  # the number of result lists
        res = [[] for _ in range(num)]

        for i, matrix in enumerate(resList):
            for line in matrix:
                res[i].append("".join(line))

        return res
```

超时了，我想也是哦。

```python
import copy


class Solution:
    def solveNQueens(self, n: int):

        chess = [['.' for _ in range(n)] for _ in range(n)]
        resList = []

        def is_valid(row, column):
            for i in range(n):
                if chess[i][column] == 'Q':
                    return False
                if chess[row][i] == 'Q':
                    return False

            for i in range(n):
                for j in range(n):
                    if abs(i - row) == abs(j - column) and chess[i][j] == 'Q':
                        return False
            return True

        def trackback(row=0):
            if row == n:
                resList.append(copy.deepcopy(chess))
                return

            for column in range(n):
                if is_valid(row, column):
                    chess[row][column] = 'Q'
                    trackback(row=row+1)
                    chess[row][column] = '.'

        trackback()
        num = len(resList)  # the number of result lists
        res = [[] for _ in range(num)]

        for i, matrix in enumerate(resList):
            for line in matrix:
                res[i].append("".join(line))

        return res
```

AC啦，但是效率比较低。

## Reflection & Polishing-up

小优化：

```python
class Solution:
    def solveNQueens(self, n: int):

        chess = [['.' for _ in range(n)] for _ in range(n)]
        resList = []

        def is_valid(row, column):
            for i in range(n):
                if chess[i][column] == 'Q':
                    return False
                if chess[row][i] == 'Q':
                    return False

            for i in range(n):
                for j in range(n):
                    if abs(i - row) == abs(j - column) and chess[i][j] == 'Q':
                        return False
            return True

        def trackback(row=0):
            if row == n:
                resList.append(["".join(line) for line in chess])
                return

            for column in range(n):
                if is_valid(row, column):
                    chess[row][column] = 'Q'
                    trackback(row=row+1)
                    chess[row][column] = '.'

        trackback()
        return resList
```

看到了一个有意思的。

```python
class Solution:
    def __init__(self):
        self.solutions = []
        self.taken = set()
    
    def solveNQueens(self, n: int) -> List[List[str]]:
        if n == 0: return []
        m = [['.' for _ in range(n)] for _ in range(n)]
        self.place_queen(0, 0, m)
        return self.solutions
    
    def place_queen(self, r, queens, m):
        if queens == len(m): 
            self.solutions.append([''.join(row) for row in m])
            return None

        for c in range(len(m[0])):
            curr_loc = {'r'+str(r),'c'+str(c),'sub_diag'+str(r-c), 'add_diag'+str(r+c)}
            if not curr_loc.intersection(self.taken):
                self.taken.update(curr_loc)
                m[r][c] = 'Q'
                self.place_queen(r+1, queens+1, m)
                m[r][c] = '.'
                self.taken.difference_update(curr_loc)
```

这里使用了一个set`curr_loc`来记录chess的信息。用到了我没想到的办法：

- 这里主要需要讲解的可能就是`'sub_diag'和'add_diag'`这两项，这个是用来验证对角线上是否有queen的参数。可以自己验证一下。
- set1.difference_update(set2)用来移除set1和set2的公共子集。
- 优点：不用每次判断是否Valid都需要遍历一次全chess。其实一开始的时候，我也是最烦如何解决斜线。

## Conclusion

- 之前在写数独的时候，也是因为回溯的东西忙的焦头烂额，现在能够这么熟练也多亏了当初。
- 之前看到有知乎还是简书上面有人说，应该将题目分类，一个类一个类的做，我个人到目前为止觉得无所谓吧，就跟深度优先和广度优先一样，更何况这个应该是个随机。
- 好像已经做了50多题了，也就是将近或是超过一个月的时间日更，我觉得还是有点成就感的，希望能继续保持。