# Leetcode 日记8

## Description

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

- Only the space character `' '` is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31, 2^31 − 1]. If the numerical value is out of the range of representable values, INT_MAX (2^31 − 1) or INT_MIN (−2^31) is returned.



### 我的解法

没什么好说的...主要就是利用RE（正则表达式）匹配字符。说实话，博主的正则表达式也没有多熟练，所以写起来挺乱的，这已经是整理过的了。

主要思路就是，先用strip()来将字符串两边的空格去掉，然后进行匹配。规则是首字为0或1个”+“或”-“，然后匹配1个或多个数字。最后记得int范围和输出的结果。

> 比如当首次匹配的不是数字也不是正负号时，返回 0

```python
import re
class Solution:
    def myAtoi(self,str):
        s = str.strip()
        pattern = re.compile(r"^[\+,\-]?[0-9]+")
        res = pattern.findall(s)
        rangemax = 2**31-1
        rangemin = -1*2**31
        if res:
            if int(res[0]) > rangemax:
                return rangemax
            elif int(res[0]) < rangemin:
                return rangemin
            else:
                return int(res[0])
        else:
            return 0
```

### 优化和思考

自己写的一个是一个非常像shell的RE表达式，确实，个人接触python时间不长，所以还不是非常pythonic，而且之前用RE更多的是在shell中使用，这次leetcode没有给solution，~~可能是因为solution太多了~~，去讨论区看了一下，确实有更优的solution。

```python
import re

class Solution:
    def myAtoi(self, s: str) -> int:
        
        num = re.match(r'\s*([+-]?\d+).*', s)        
        if num:
            res = int(num.group(1))
            
            if res<= -2**31: res=-2**31
            elif res >= 2**31-1: res=2**31-1
            
            return res
        else:
            return 0
```

我们先来看一下，这个是怎么实现的，其实差异在这一段代码`num = re.match(r'\s*([+-]?\d+).*', s)`。

`\s`代表的是任意空白字符，`\d`代表任意数字等价`[0-9]`，`.*`匹配任意字符。

中间的这个括号内的内容跟我的表达式是一样的，但是这个括号使得这个匹配的字段分为了两个group：

- group(0):可能包含空白以及数字后内容的字符串。
- group(1):括号内的字符串。

这也就是后面两行`res = int(num.group(1))`这段代码的前驱。确实比我自己写的更pythonic，更巧妙，耗时也更少。