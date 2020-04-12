# Leetcode No.79 Word Search

## Description

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

## My Solution

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        length = len(board)
        if not length and word:
            return False
        width = len(board[0])

        route = []

        def backtrack(index, row, column):
            if index == len(word):
                return True

            if (row, column) in route:
                return False

            if row >= length or column >= width or row < 0 or column < 0:
                return False

            if board[row][column] == word[index]:
                route.append((row,column))
                ans = backtrack(index + 1, row - 1, column) or backtrack(index + 1, row + 1, column) or backtrack(index + 1, row, column - 1) or backtrack(index + 1, row, column + 1)
                if ans:
                    return True
                route.remove((row,column))
                return False

        pos = []
        for i, line in enumerate(board):
            for j, item in enumerate(line):
                if board[i][j] == word[0]:
                    pos.append((i, j))

        for row, column in pos:
            if backtrack(0, row, column):
                return True

        return False
```

很麻烦的回溯算法。因为不能走之前走过的路线。

## Reflection & Polishing-up

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        r = len(board)
        c = len(board[0])
        
        def isValid(i,j,idx, visited):
            if (i,j) in visited:
                return False

            # print(visited, idx)
            if idx == len(word):
                return True
            elif i<0 or i>=r or j<0 or j>=c:
                return False
            elif board[i][j] == word[idx]:
                return isValid(i+1, j, idx+1, visited+[(i,j)]) or isValid(i-1, j, idx+1,visited+[(i,j)]) or isValid(i, j+1, idx+1,visited+[(i,j)]) or isValid(i, j-1, idx+1,visited+[(i,j)])
            else:
                return False
        
        for i in range(r):
            for j in range(c):
                if board[i][j] == word[0]:
                    if isValid(i,j,0,[]): return True
        return False
```

直接用visited参数来判断是否已经走过了，比我用一个函数内的变量route来存储更优。

## Conclusion

- 这算第二次对回溯中间的过程感到苦手了吧。第一次是刚刚接触回溯法写数独那会。
- 学无止尽，应当精益求精，不能满足于当下。
- stay hungry, stay foolish.