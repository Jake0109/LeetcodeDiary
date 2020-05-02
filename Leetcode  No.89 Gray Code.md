# Leetcode  No.89 Gray Code

## Description

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer *n* representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

**Example 1:**

```
Input: 2
Output: [0,1,3,2]
Explanation:
00 - 0
01 - 1
11 - 3
10 - 2

For a given n, a gray code sequence may not be uniquely defined.
For example, [0,2,3,1] is also a valid gray code sequence.

00 - 0
10 - 2
11 - 3
01 - 1
```

**Example 2:**

```
Input: 0
Output: [0]
Explanation: We define the gray code sequence to begin with 0.
             A gray code sequence of n has size = 2n, which for n = 0 the size is 20 = 1.
             Therefore, for n = 0 the gray code sequence is [0].
```

## My Solution

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        
        def b2d(code):
            res = 0
            for i in range(len(code)):
                res += code[i] * 2**i
            return res

        if n == 0:
            return [0]

        code = [0] * n
        codes = set()
        res = [0]
        codes.add(b2d(code))

        def changeElement(j):
            if code[j] == 1:
                code[j] = 0
            else:
                code[j] = 1

        for i in range(2**n):
            for j in range(n):

                changeElement(j)

                de = b2d(code)

                if de not in codes:
                    codes.add(de)
                    res.append(de)
                    break
                else:
                    changeElement(j)

        return res
```

虽然AC了，但这代码肯定要改...

## Reflection & Polishing-up

使用了Python内置的二进制运算。

```python
class Solution:
    def grayCode(self, n: int):

        bincode = "0b" + "0"*n
        res = [0]

        def changeCode(j):
            code = list(bincode)
            if code[j] == "1":
                code[j] = "0"
            else:
                code[j] = "1"
            return "".join(code)

        for i in range(1,2**n-1):
            temp = bin(i)
            for j in range(len(temp)-1,1,-1):
                # run this line to find the bug
                print(f"temp:{temp} bincode:{bincode}")
                if temp[j] == "1":
                    bincode = changeCode(j)
                    res.append(int(bincode,2))
                    break
        return res
```

偷懒不成功，至于为什么，可以自己跑一遍，我把需要看的输出已经写出来了。改吧...

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        
        bincode = "0b" + "0"*n
        res = [0]

        def changeCode(j):
            code = list(bincode)
            if code[j] == "1":
                code[j] = "0"
            else:
                code[j] = "1"
            return "".join(code)

        for i in range(1,2**n):
            temp = bin(i)
            for j in range(len(temp)-2):
                index = -j-1
                if temp[index] == "1":
                    bincode = changeCode(index)
                    res.append(int(bincode,2))
                    break
        return res
```

第一版不成功的原因在于，bin()函数并不会直接将0转化成"0b000"这种形式，所以导致了index的错误，如果能够指定二进制的长度的话，需要自己重新写代码，这可就太麻烦了...于是就妥协使用了这种方式。

其他讨论区里面的也都大差不差，就不在这里多一嘴了。

## Conclusion

- 其实做这种题目，对于我个人而言是最快乐的，因为能够切实的感受到自己从没思路到有思路；再从一般的思路想到一个更优的思路，并且将其一一实现。
- 我刷题就是为了个人能力的提升，而不是用刷题量来证明自己。所以要认清自己做这一件事的目的是什么，所谓的不忘初心也不过如此，能够完成需求，达到目的，过程不过是锦上添花。