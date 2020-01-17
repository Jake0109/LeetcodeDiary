# Leetcode No.12 Integer to Roman

## Description

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

## My Solution

这个虽然是个medium，但是我感觉我真的花了不少时间在上面。

对每一位进行判断，决定最后的输出结果。

分为四种情况：0-3，4，5-8，9。

每种情况的解决方法都不一样。具体看代码吧。

有一个carry进位符，来决定当前使用哪一套符号，比如[I,V],[X,L]...

```python
class Solution:
    def intToRoman(self,num):
        ten_list = ['I','X','C','M']
        five_list = ['V','L','D']
        arr = []
        res_list = []
        while num>0:
            arr.append(num%10)
            num = num // 10

        carry = 0
        for i in arr:
            if i in range(4):
                for j in range(i):
                    res_list.append(ten_list[carry])
            elif i == 4:
                res_list.append(five_list[carry])
                res_list.append(ten_list[carry])
            elif  i in range(5,9):
                for j in range(i-5):
                    res_list.append(ten_list[carry])
                res_list.append(five_list[carry])
            else:
                res_list.append(ten_list[carry+1])
                res_list.append(ten_list[carry])
            carry += 1
        res_list.reverse()
        return "".join(res_list)
```

应该是个侥幸啊，最后的结果竟然：

Runtime: 40 ms, faster than 91.56% of Python3 online submissions for Integer to Roman.

Memory Usage: 12.6 MB, less than 100.00% of Python3 online submissions for Integer to Roman.

~~我觉得我可能发现了为什么我比大多数快的原因了，因为没有官方的solution~~

## Reflection & Polishing-up

感觉对这个问题想的时间太多了，其实真正做起来还挺流畅的。光是想花费了太多的时间，说不定上手做做意外的会很快。思而不学则殆。

然后最大的问题就是这个代码太长了...其实思路其实不难，但总感觉应该能有更优，更pythonic的解决方案，比如用字典之类的。尝试了一下，用key为字符value为数量的方法，又苦于寻找正确value的路上，尤其是关系到9和4的时候。于是我就决定去讨论区看看。果然看到了很有趣，而且很优雅的代码：

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        roman = ""
        rv = {'M':1000,'CM':900, 'D':500,'CD':400,'C':100, 'XC':90
              ,'L':50,'XL':40, 'X':10, 'IX':9, 'V':5, 'IV':4, 'I':1}
        i = 0
        while(num>0):
            if(num - list(rv.values())[i] >= 0 ):
                roman += (list(rv.keys())[i])
                num -= list(rv.values())[i]
            else: 
                i += 1
        return roman
```

一方面，这个解决思路真的非常有趣，是一个通过减法实现的solution，而且`list(dict.values()[i])`这种操作我现在不会，这次记下了，在dict中的遍历，确实很很有意思。我暂时想不出来能对这段代码进行什么优化。因为对我而言，它确实太优雅，太优秀了。