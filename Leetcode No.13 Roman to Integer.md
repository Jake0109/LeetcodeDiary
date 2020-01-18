# Leetcode No.13 Roman to Integer

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

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

## My Solution

本来可以用很多if...elif...elif...else来很简单，很无脑的完成这个操作，だが、断る！

昨天看了用字典的那个骚操作，自己也想试试用字典的骚操作，于是就有了如下的代码：

```python
class Solution:
    def romanToInt(self,s):
        plus_dict = {'M': 1000, 'D': 500, 'C': 100, 'L': 50, 'X': 10, 'V': 5,
                'I': 1}
        minus_dict = {'CM':200,'CD':200,'XC':20,'XL':20,'IX':2,'IV':2}
        res = 0
        for j in range(len(s)):
            for i in plus_dict.keys():
                if i == s[j]:
                    res += plus_dict[i]

        for i in minus_dict.keys():
            if i in s:
                res -= minus_dict[i]
        return res
```

~~但是总感觉，没内味~~试着优化一下：

```python
def romanToInt(s):
    plus_dict = {'M': 1000, 'D': 500, 'C': 100, 'L': 50, 'X': 10, 'V': 5,
            'I': 1}
    minus_dict = {'CM':200,'CD':200,'XC':20,'XL':20,'IX':2,'IV':2}
    res = 0
    for j in range(len(s)):
        if "".join(s[j:j+2]) in minus_dict:
            res -= minus_dict["".join(s[j:j+2])]
        for i in plus_dict.keys():
            if i == s[j]:
                res += plus_dict[i]
    return res
```

结果运行的更慢了...

## Reflection & Polishing-up

无可奈何的我，就去讨论区看看大神们是怎么写的吧。果不其然，看到了差距。

```python
def romanToIntII(self, s: str) -> int:
        hash = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
        n = len(s)

        ans = 0

        for i in range(n):
            ans = ans - hash[s[i]] if i < n - 1 and hash[s[i]] < hash[s[i+1]] else ans + hash[s[i]]

        return ans
```

这个人一看就知道是大神，首先从格式来看，大概是个直接在浏览器里面敲代码的，肯定对这种代码轻车熟路，然后定义的字典名称为hash，想必也是熟知，python字典的本质就是hash表。再看看这个嵌套判断的赋值语句，都是我使不好的骚操作，~~或者说现阶段根本不会使~~而且代码效率也比我的高。

路漫漫其修远兮，吾将上下而求索。