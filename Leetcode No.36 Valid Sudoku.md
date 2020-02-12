# Leetcode No.36 Valid Sudoku

## Description

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

**Example 1:**

```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

**Example 2:**

```
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.
- The given board contain only digits `1-9` and the character `'.'`.
- The given board size is always `9x9`.

## My Solution

犯难了...我...几乎没玩过数独啊...明天的题目是解数独啊...话说，数独原来是源自日本的游戏啊...

```python
class Solution:
    def isValidSudoku(self,board):
        # line
        for line in board:
            dict = {}
            for num in line:
                if not num.isdigit():
                    continue
                if num not in dict:
                    dict[num] = 1
                else:
                    return False

        # column
        for i in range(9):
            dict = {}
            for line in board:
                if not line[i].isdigit():
                    continue
                if line[i] not in dict:
                    dict[line[i]] = 1
                else:
                    return False

        # matrix
        for i in range(0,10,3):
            for j in range(0,10,3):
                dict = {}
                for line in board[i:i+3]:
                    for num in line[j:j+3]:
                        if not num.isdigit():
                            continue
                        if num not in dict:
                            dict[num] = 1
                        else:
                            return False


        return True    
```

我也写了注释了...就是行列，3*3，明天是真的不知道该咋整了，今天没事的时候自己做做数独找找感觉吧。

## Reflection & Polishing-up

去看了一下讨论区，也没什么太大的收获，大家都差不多...

## Conclusion

- 在英文中，行列应该用 row,column 来表示，以后我改。
- 像这里的3*3，用cell表示更让人理解。我以后碰到matrix的题，我也这么做。