# Leetcode 日记7

## Description

Given a 32-bit signed integer, reverse digits of an integer.

### 解法

很简单，这里就直接放代码了

```python
class Solution:
    def reverse(self,x):
        if x < -2147483648 or x > 2147483647:
            return 0
        res = 0
        temp = abs(x)
        while temp != 0:
            res *= 10
            res += temp % 10
            temp = int(temp/10)
        
        if res < -2147483648 or res > 2147483647:
            return 0
        if x > 0:
            return res
        else:
            return -res
```



### 优化与反思

首先是python的Number类型。

对于python来说，它不像java等语言，int的长度为32位，long的长度为64位。

当使用`var= = 10`这样的语句对一个变量进行赋值的时候，它就是一个Number类型的变量，长度只和内存大小有关，只要不超过内存容量，这个变量可以非常非常大。所以我们需要对这个函数的取值进行限制。

再一点：上面的是最general的解法，那么在用python来解的时候，有没有更pythonic的解法呢。有的。

```python
class Solution:
    def reverse(self, x: int) -> int:  
        if x > 0: 
            a =  int(str(x)[::-1])  
        if x <=0: 
            a = -1 * int(str(x*-1)[::-1])  
        rangemin = -2**31  
        rangemax = 2**31 - 1  
        if a not in range(mina, maxa):  
            return 0  
        else:  
            return a
```

其实我觉得可以再优化一下，就是如果输入的就在32位的范围外的话，可以一开始就直接返回0。

```python
class Solution:
    def reverse(self, x: int) -> int:
        rangemin = -2**31  
        rangemax = 2**31 - 1  
        if x not in range(rangemin,rangemax):
            return 0
        if x > 0: 
            a =  int(str(x)[::-1])  
        if x <=0: 
            a = -1 * int(str(x*-1)[::-1])  
        if a not in range(rangemin, rangemax):  
            return 0  
        else:  
            return a
```

不过考虑到别的语言，如果输入的x超过了int的范围，应该就直接报错了...可能对于一个多语言的OJ来说，这个行为可能多此一举来了...