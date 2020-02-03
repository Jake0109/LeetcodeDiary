# Leetcode No.28 Implement strStr()

## Description

Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/) and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).

## My Solution

利用现有的函数的话，这样就行了：

```python
class Solution:
    def strStr(self,haystack: str, needle: str) -> int:
        if not needle:
            return 0
        elif needle not in haystack:
            return -1
        else:
            return haystack.index(needle)
```

然后，自己写算法实现的话就这样：

```python
class Solution:
    def strStr(self,haystack: str, needle: str) -> int:
        if not needle:
            return 0
        length = len(needle)
        for i in range(len(haystack)):
            if haystack[i:i+length] == needle:
                return i
        else:
            return -1
```

比用现成函数的慢...

## Reflection & Polishing-up

一个one-line代码，忘了find()了，挺好的，让我回忆起来了。

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```

>  这里提一下，这里一行代码的实现不能直接用index()哦，因为如果needle不在haystack里面的话，index()函数会报错，而不是返回-1。

这里把str.index()和str.find()函数贴出来。

```python
 |  index(...)
 |      S.index(sub[, start[, end]]) -> int
 |      
 |      Return the lowest index in S where substring sub is found, 
 |      such that sub is contained within S[start:end].  Optional
 |      arguments start and end are interpreted as in slice notation.
 |      
 |      Raises ValueError when the substring is not found.

 |  find(...)
 |      S.find(sub[, start[, end]]) -> int
 |      
 |      Return the lowest index in S where substring sub is found,
 |      such that sub is contained within S[start:end].  Optional
 |      arguments start and end are interpreted as in slice notation.
 |      
 |      Return -1 on failure.
```



没找到特别好的，除了几个用别的语言看不懂的...一个是用KMP实现的，稍微找了点资料看了一下，