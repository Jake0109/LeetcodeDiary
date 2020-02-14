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

今天重构了一下这一段代码，现在大概是这个样子的：

```python
class Solution:
    def solveSudoku(self,board):
        def is_valid(data,row,column):
            data = str(data)
            for i in range(9):
                if board[i][column] == data:
                    return False
                if board[row][i] == data:
                    return False
            
            sub_row = row // 3 * 3
            sub_column = column //3 * 3
            for i in range(sub_row,sub_row+3):
                for j in range(sub_column,sub_column+3):
                    if board[i][j] == data:
                        return False
            return True

        def traceback(index):
            row = empty_cells[index][0]
            column = empty_cells[index][1]
            for num in range(1,10):
                if is_valid(num,row,column):
                    board[row][column] = str(num)
                    index += 1
                    
                    pass
                    
                    board[row][column] = '.'
                    index -= 1
        
        empty_cells = []
        for i,line in enumerate(board):
            for j,element in enumerate(line):
                if element == '.':
                    empty_cells.append([i,j])
        
        traceback(0)
```

一个是没有测试，另外，中间那个大大的pass，当然是没做好的。

```python
class Solution:
    def solveSudoku(self,board):
        def is_valid(data,row,column):
            data = str(data)
            for i in range(9):
                if board[i][column] == data:
                    return False
                if board[row][i] == data:
                    return False

            sub_row = row // 3 * 3
            sub_column = column //3 * 3
            for i in range(sub_row,sub_row+3):
                for j in range(sub_column,sub_column+3):
                    if board[i][j] == data:
                        return False
            return True

        def traceback(index):
            row = empty_cells[index][0]
            column = empty_cells[index][1]
            for num in range(1,10):
                if is_valid(num,row,column):
                    board[row][column] = str(num)

                    if index == len(empty_cells) - 1:
                        self.flag = 1
                        return
                    if index < len(empty_cells) - 1:
                        traceback(index + 1)
                        
                    if self.flag == 0:
                        board[row][column] = '.'

        empty_cells = []
        for i,line in enumerate(board):
            for j,element in enumerate(line):
                if element == '.':
                    empty_cells.append([i,j])

        self.flag = 0
        traceback(0)
```

我决定把中间那段整合成这样：

```python
        def traceback(index):
            row = empty_cells[index][0]
            column = empty_cells[index][1]
            for num in range(1,10):
                if is_valid(num,row,column):
                    board[row][column] = str(num)

                    if index == len(empty_cells) - 1:
                        self.flag = 1
                        return
                    if index < len(empty_cells) - 1:
                        traceback(index + 1)
                        
                    if self.flag == 0:
                        board[row][column] = '.'
```

因为我认为没有必要再函数里面就改变index的值，不如将index+1作为参数传递给递归函数。



## Reflection & Polishing-up

好像这一篇的篇幅比较大，虽然大多数都是错的...但是总归是做出来了。虽然参考了别人的，但是一个本来就不怎么会玩数独的人，写了个东西自动化搞数独，不也是挺浪漫的吗。~~话说今天情人节，大家过得怎么样啊>.<~~还是多去看看别人是怎么写的吧。听说还是有不少更优的解法的。

一个用set的解法：

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        row = [set(range(1, 10)) for _ in range(9)]  # 行剩余可用数字
        col = [set(range(1, 10)) for _ in range(9)]  # 列剩余可用数字
        block = [set(range(1, 10)) for _ in range(9)]  # 块剩余可用数字

        empty = []  # 收集需填数位置
        for i in range(9):
            for j in range(9):
                if board[i][j] != '.':  # 更新可用数字
                    val = int(board[i][j])
                    row[i].remove(val)
                    col[j].remove(val)
                    block[(i // 3)*3 + j // 3].remove(val)
                else:
                    empty.append((i, j))

        def backtrack(iter=0):
            if iter == len(empty):  # 处理完empty代表找到了答案
                return True
            i, j = empty[iter]
            b = (i // 3)*3 + j // 3
            for val in row[i] & col[j] & block[b]:
                row[i].remove(val)
                col[j].remove(val)
                block[b].remove(val)
                board[i][j] = str(val)
                if backtrack(iter+1):
                    return True
                row[i].add(val)  # 回溯
                col[j].add(val)
                block[b].add(val)
            return False
        backtrack()
```

看完发现，丢人了...回溯的英文是backtrack不是traceback...妈耶...

- `for val in row[i] & col[j] & block[b]:`还能有这种用法...我都是分开来写三个in。
- 这里将block这个线性化了，不像我的代码里，是`cell[0][0]cell[0][1]...`这个样子的，确实也非常方便。

最初我看到的solution跟这个差不多，所以一开始我的solution跟这个有点像，都是用set的。但是毕竟不懂回溯，也没理清思路，所以写不出来。现在看的话，清晰很多。

## Conclusion

- 学会多种渠道获取信息，在上一个模块中给大家display的solution显然不是官方的leetcode的discussion里面的，是leetcode国内讨论区里面的，兼听则明。
- 英语还是要继续学，backtrack之类的...对名命有很大的帮助，从而对写出易于理解的代码也有很大帮助。
- 最近看了看python的PEP 8，还在继续阅读中，代码规范非常重要，我觉得我重构的代码就比一开始写的要好，从结构上来说。想我这种没怎么搞过项目的，代码规范可能会成为减分项，所以现在查漏补缺吧。