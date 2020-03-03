# Leetcode No.52 N-Queen II

## Description

The *n*-queens puzzle is the problem of placing *n* queens on an *n*×*n* chessboard such that no two queens attack each other.

![img](https://assets.leetcode.com/uploads/2018/10/12/8-queens.png)

Given an integer *n*, return the number of distinct solutions to the *n*-queens puzzle.

**Example:**

```
Input: 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown below.
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

## My Solution

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        chessboard = set()
        res = []

        def trackback(row=0):
            if row == n:
                res.append(chessboard)
            for column in range(n):
                temp_set = {'row:{}'.format(row), 'colunm:{}'.format(column), 										'row+colunm:{}'.format(row + column),
                            'row-colunm{}'.format(row - column)}
                if not chessboard.intersection(temp_set):
                    chessboard.update(temp_set)
                    trackback(row=row + 1)
                    chessboard.difference_update(temp_set)

        trackback()
        return len(res)
```

借鉴了昨天那个比较数学的解决方案，能不去看自己写出来，还是挺好的。但是为什么还是很慢呢...

## Reflection & Polishing-up

看到一个挺有意思的：

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        self.ans = 0
        def nQueenBitset(n,i,c,d1,d2):
            if i == n:
                self.ans += 1
                return
            for j in range(n):
                if not c[j] and not d1[i-j+n+1] and not d2[i+j]:
                    c[j] = d1[i-j+n+1] = d2[i+j] = True
                    nQueenBitset(n,i+1,c,d1,d2)
                    c[j] = d1[i-j+n+1] = d2[i+j] = False
        c = [False for i in range(n)]
        d1 = [False for i in range(2*n+1)]
        d2 = [False for i in range(2*n+1)]
        nQueenBitset(n,0,c,d1,d2)
        return self.ans
```

用的c,d1,d2这3个分别来存储行占用和两个斜线的占用情况。确实在不用给出具体chessboard的时候，需要想着能够继续偷懒的办法才行。

另外一个类似的是这个：

```python
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
                
        occupiedCol = set()
        occupiedDiag = set()
        occupiedDiagI = set()
        
        
        self.ans = 0 
        def dfs(row):
            if row == n:
                self.ans += 1
                return 
            for j in range(n):
                if j not in occupiedCol and row+j not in occupiedDiag \
                    and row-j not in occupiedDiagI:
                    ####### Backtracking ############
                    occupiedCol.add(j)
                    occupiedDiag.add(row+j)
                    occupiedDiagI.add(row-j)
                    dfs(row+1)
                    occupiedCol.remove(j)
                    occupiedDiag.remove(row+j)
                    occupiedDiagI.remove(row-j)
                    ####### Backtracking ############
        
        dfs(0)
        return self.ans
```

是个用set的方法，我觉得也是OK的，只不过每次set插入的操作应该是比list赋值是要更加花费时间一些。原来这种斜线/对角线叫做 diagonal 啊哈哈哈。

## Conclusion

- 今天那算是比昨天进步了一点，但是还远远不够，还有更多优秀的解法在那里等待发现。
- 铭记乔布斯那句话：stay hungry, stay foolish.