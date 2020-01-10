# Leetcode 日记6

## Zigzag conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```python
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

这个问题最形象的解法，就是先将每个char放入对应的位置，然后按行遍历得出结果，这个有点太直接，博主就不写了，博主觉得，数学，计算机教我们的，出了单纯的coding，以及各种各样的structure之外，最重要的就是抽象的能力。如何把一个很形象的问题，抽象成数学问题来求解，是一个计算机学生非常重要的能力。

### 第一版

在这个是实现方法里面，我们把整个s的index分为了两个分子，loop和length，在当前这个理想的状态下`len(s)=loop*length`，这个函数就能完成需求。

```python
def convert(s,n):
    loop = 2*n-2
    length = int(len(s)/loop)
    arr = []
    if len(s) % loop != 0:
        length += 1

    for i in range(n):
        for j in range(length):
           if i == 0 or i == n-1:
                arr.append(s[j*loop+i])
           else:
                arr.append(s[j*loop+i])
                arr.append(s[(j+1)*loop-i])
    return "".join(arr)
```

这里注意一下 join() 这个函数，它的用法是`"sep".join(seq)`。这个函数的功能是将一个迭代器seq以一个sep为中间符号，整合成一个字符串。这里用到了这个函数直接返回一个字符串结果（题目要求）。

### 加入条件判断，终端循环

```python
def convert(s,n):
    loop = 2*n-2
    length = int(len(s)/loop)
    arr = []
    if len(s) % loop != 0:
        length += 1

    for i in range(n):
        for j in range(length):
            if i == 0 or i == n-1:
                if j*loop+i >= len(s):
                    break
                arr.append(s[j*loop+i])
            else:
                if j*loop+i >= len(s):
                    break
                arr.append(s[j*loop+i])
                if (j+1)*loop-i >= len(s):
                    continue
                arr.append(s[(j+1)*loop-i])
    return "".join(arr)
```

失误：上面当n=1时，会出现除数为0的情况，会报错。

```python
class Solution:
    def convert(self,s,n):
        if n == 1:
            return s
        loop = 2*n-2
        length = int(len(s)/loop)
        arr = []
        if len(s) % loop != 0:
            length += 1

        for i in range(n):
            for j in range(length):
                if i == 0 or i == n-1:
                    if j*loop+i >= len(s):
                        break
                    arr.append(s[j*loop+i])
                else:
                    if j*loop+i >= len(s):
                        break
                    arr.append(s[j*loop+i])
                    if (j+1)*loop-i >= len(s):
                        continue
                    arr.append(s[(j+1)*loop-i])
        
        return "".join(arr)
```

### 另外的解法

```python
class Solution(object):
    def convert(self, s, numRows):
        if numRows == 1:
            return s 
        n = len(s)
        cycle = 2*numRows - 2
        strlist = []
        for i in range(numRows):
            for j in range(i, n, cycle):
                strlist.append(s[j])
                if i != numRows-1 and i != 0 and j+cycle-2*i < n:
                    strlist.append(s[j+cycle-2*i])             
        newstr = ''.join(strlist)
        return newstr
```

看了看一些python的solution，这个确实比较有意思，巧妙利用了range()函数的特性，用一个loop作为步数来进行遍历，这样就不需要像我的solution那样添加那么多 break 和 continue，代码更简洁。