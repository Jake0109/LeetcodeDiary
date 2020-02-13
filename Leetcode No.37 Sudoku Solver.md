# Leetcode No.37 Sudoku Solver

## Description

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

Empty cells are indicated by the character `'.'`.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
A sudoku puzzle...

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
...and its solution numbers marked in red.

**Note:**

- The given board contain only digits `1-9` and the character `'.'`.
- You may assume that the given Sudoku puzzle will have a single unique solution.
- The given board size is always `9x9`.

## My Solution

没错，我还是一个连手解数独都一塌糊涂的人，开始用电脑自动化解数独了！不晓得今天能不能成。看了一下，好像用最多的是回溯法，然而我不会啊...~~你不能学吗！~~我就先慢慢来吧。

```python
def solvesudoku(board):
    row = [set() for i in range(9)]
    column = [set() for i in range(9)]
    cell = [[set() for i in range(3)] for i in range(3)]

    for i, line in enumerate(board):
        for j, element in enumerate(line):
            if element != '.':
                row[i].add(element)
                column[j].add(element)
                cell[i // 3][j // 3].add(element)

    order = sorted(list(range(9)), key=lambda x: len(row[x]), reverse=True)

    i,j = 0,0
    while i<9 and j < 9:
        if board[i][j] == '.':
            for x in range(1,10):
                x = str(x)
                if x not in row[i] and x not in column[j] and x not in cell[i//3][j//3]:
                    board[i][j] = x
                    row[i].add(x)
                    column[j].add(x)
                    cell[i//3][j//3].add(x)
                    break
        if j <8:
            j += 1
        else:
            j = 0
            i += 1

    for line in board:
        print(line)
```

显然是有问题的...比如说如下的输入和输出：

```python
[["5", "3", ".", ".", "7", ".", ".", ".", "."],
 ["6", ".", ".", "1", "9", "5", ".", ".", "."],
 [".", "9", "8", ".", ".", ".", ".", "6", "."],
 ["8", ".", ".", ".", "6", ".", ".", ".", "3"],
 ["4", ".", ".", "8", ".", "3", ".", ".", "1"],
 ["7", ".", ".", ".", "2", ".", ".", ".", "6"],
 [".", "6", ".", ".", ".", ".", "2", "8", "."],
 [".", ".", ".", "4", "1", "9", ".", ".", "5"],
 [".", ".", ".", ".", "8", ".", ".", "7", "9"]]

['5', '3', '1', '2', '7', '4', '8', '9', '.']
['6', '2', '4', '1', '9', '5', '3', '.', '7']
['.', '9', '8', '3', '.', '.', '1', '6', '2']
['8', '1', '2', '5', '6', '7', '4', '.', '3']
['4', '5', '6', '8', '.', '3', '7', '2', '1']
['7', '.', '3', '9', '2', '1', '5', '.', '6']
['1', '6', '5', '7', '3', '.', '2', '8', '4']
['2', '7', '.', '4', '1', '9', '6', '3', '5']
['3', '4', '.', '6', '8', '2', '.', '7', '9']
```

那么，判断是否回溯的点，应该在这里：

```python
    while i<9 and j < 9:
        if board[i][j] == '.':
            for x in range(1,10):
                x = str(x)
                if x not in row[i] and x not in column[j] and x not in cell[i//3][j//3]:
                    board[i][j] = x
                    row[i].add(x)
                    column[j].add(x)
                    cell[i//3][j//3].add(x)
                    break
			else:#这里是判断回溯的点
                pass
        if j <8:
            j += 1
        else:
            j = 0
            i += 1
```

那么问题来了，怎么判断是回溯到上一步，两步还是三步，甚至一开始的点就错了呢？这我就又懵了...

```python
def solvesudoku(board):
    row = [set() for i in range(9)]
    column = [set() for i in range(9)]
    cell = [[set() for i in range(3)] for i in range(3)]

    empty_cells = []
    for i, line in enumerate(board):
        for j, element in enumerate(line):
            if element != '.':
                row[i].add(element)
                column[j].add(element)
                cell[i // 3][j // 3].add(element)
            else:
                empty_cells.append([i, j])

    # order = sorted(list(range(9)), key=lambda x: len(row[x]), reverse=True)

    def isValid(index, data):
        row, column = empty_cells[index][0], empty_cells[index][1]
        data = str(data)
        if data in board[row]:
            return False
        for x in [board[i][column] for i in range(9)]:
            if data == x:
                return False


        for i in range(row//3*3, row // 3 + 3):
            for j in range(column // 3 * 3, column // 3 + 3):
                if data == board[i][j]:
                    return False

        return True

    def traceBack(index):

        for data in range(1, 10):
            if isValid(index, data):
                board[empty_cells[index][0]][empty_cells[index][1]] = str(data)
                index += 1
                for line in board:
                    print(line)
                print(empty_cells[index-1])


                if index == len(empty_cells)-1:
                    return True

                elif index + 1 < len(empty_cells):
                    traceBack(index)

                if flag == 0:
                    print("traceback",board[empty_cells[index][0]][empty_cells[index][1]])
                    board[empty_cells[index][0]][empty_cells[index][1]] = '.'


    index = 0
    flag = 0
    traceBack(index)
```

写成这样了以后...我懵了，明天继续。

