# Leetcode No.67 Add Binary

## Description

Given two binary strings, return their sum (also a binary string).

The input strings are both **non-empty** and contains only characters `1` or `0`.

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

## My Solution

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:

        def add(aNum, bNum, c):
            if aNum + bNum + c == "000":
                return "0", "0"
            elif aNum + bNum + c in ["001", "010", "100"]:
                return "1", "0"
            elif aNum + bNum + c in ["011", "101", "110"]:
                return "0", "1"
            else:
                return "1", "1"

        aNums = list(a)
        bNums = list(b)
        res = []
        c = "0"
        while aNums and bNums:
            num, c = add(aNums.pop(), bNums.pop(), c)
            res.append(num)
        print(res)
```

感觉不太对，重新写一遍：

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        
        def add(aNums, bNums, c):
            aNum = aNums.pop() if aNums else "0"
            bNum = bNums.pop() if bNums else "0"
            if aNum + bNum + c == "000":
                return "0", "0"
            elif aNum + bNum + c in ["001", "010", "100"]:
                return "1", "0"
            elif aNum + bNum + c in ["011", "101", "110"]:
                return "0", "1"
            else:
                return "1", "1"

        aNums = list(a)
        bNums = list(b)
        res = []
        c = "0"
        while aNums or bNums:
            num, c = add(aNums, bNums, c)
            res.append(num)
        if c == "1":
            res.append(c)
        res.reverse()
        return "".join(res)
```

感觉笨笨的...

## Reflection & Polishing-up

再修改一次：

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        
        aNums = list(a)
        bNums = list(b)
        res = []
        c = 0
        while aNums or bNums:
            aNum = int(aNums.pop()) if aNums else 0
            bNum = int(bNums.pop()) if bNums else 0
            sum = aNum + bNum + c
            if sum == 0:
                res.append("0")
                c = 0
            elif sum == 1:
                res.append("1")
                c = 0
            elif sum == 2:
                res.append("0")
                c = 1
            else:
                res.append("1")
                c = 1
        if c == 1:
            res.append("1")
        res.reverse()
        return "".join(res)
```

但是效率还是不高...

看到的最好的一个是这个：

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        i, j, counter = len(a) - 1, len(b) - 1, '0'
        res = []
        while(i >= 0 or j >= 0 or counter != '0'):

            num1 = a[i] if i >= 0 else '0'
            num2 = b[j] if j >= 0 else '0'

            if num1 == num2:
                res.append(counter)
                counter = num1
            else:
                res.append('1' if counter == '0' else '0')

            i -= 1 
            j -= 1 


        return ''.join(res[::-1])
```

## Conclusion

- 最近可能有点浮躁，一个是目前手上没有offer，另一个是，暂时还没有一家公司的面试通知，如果说我不烦躁的话，应该是不可能的。
- 另一方面，学校毕业设计的东西要开始花大量的实践做了，打算停一段时间Leetcode，然后可以将项目的开发进度，心得之类的传上来。