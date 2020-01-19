# Leetcode No.14 Longest Common Prefix

## description

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

## My Solution

Ver1：

```python
def longestCommonPrefix(strs):
    res = ""
    for i in range(len(strs[0])):
        for j in strs[1::]:
            if j[i] != strs[0][i]:
                return res
        res += strs[0][i]
    return res
```

问题很多，首先，它本身就不对...在每次读取完一个strs的内容以后，如果它是common prefix，它都会执行一遍`res += strs[0][i]`

就很傻。

Ver2：

```python
def longestCommonPrefix(strs):
    res = ""
    for i in range(len(strs[0])):
        for j in strs[1::]:
            if j[i] != strs[0][i]:
                return res
        else:
       		res += strs[0][i]
    return res
```

用了个很pyhonic的for...else...句式嘿嘿。

其次，如果strs这个列表为空，就会报错，目录超过范围。

Ver.3

```python
class Solution:
    def longestCommonPrefix(self,strs):
        res = ""
        if strs:
            for i in range(len(strs[0])):
                for j in strs[1::]:
                    if j[i] != strs[0][i]:
                        return res
                else:
                    res += strs[0][i]
        else:
            return ""
        return res
```

然后就是，如果strs[0]的长度比其他元素都长，并且，其他的所有元素都是strs[0]的prefix，比如这么一组`["abcd","a","ab"]`也会报目录超过范围的错误。

Ver.4

```python
class Solution:
    def longestCommonPrefix(self,strs):
        res = ""
        if strs:
            for i in range(len(strs[0])):
                for j in strs[1::]:
                    if i >= len(j) or j[i] != strs[0][i]:
                        return res
                else:
                    res += strs[0][i]
        else:
            return ""
        return res
```

那么这次就是一个可以AC的代码了，时间复杂度为O(n*m)，n=len(strs)，m=len(strs[0])。

## Reflection & Polishing-up

首先，那个判断列表是否为空的那个地方，可以稍微改进一下：

```pthon
class Solution:
    def longestCommonPrefix(self,strs):
    	if not strs:
    		return ""
        res = ""
        for i in range(len(strs[0])):
            for j in strs[1::]:
                if i >= len(j) or j[i] != strs[0][i]:
            	    return res
            else:
                res += strs[0][i]
        return res
```

那么我们来看看别的Solution吧：

```python
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if not strs: return ""
        if len(strs) == 1: return strs[0]
        
        strs.sort()
        p = ""
        for x, y in zip(strs[0], strs[-1]):
            if x == y: p+=x
            else: break
        return r
```

其实这种类似的，直接使用内置函数的代码，我个人在实现的时候不会使用，因为这个违背了我个人，为了补习算法和数据结构而来练题的初衷，但是我肯定会看，最好希望每个我刷过的题底下都有类似的solution，因为这个能让我知道一些我可能不会用，或者说没有想到要用的，那些比我自己写的代码更高效的内置函数。

看到了一段很有意思的代码：

```python
def longestCommonPrefix(strs):
    """
    :type strs: List[str]
    :rtype: str
    """
    prefix = []
    num = len(strs)
    for x in zip(*strs):
        if len(set(x)) == 1:
            prefix.append(x[0])
        else:
            break
    return "".join(prefix)
```

很pythonic，而且有一个我一直没有怎么接触到的点就是`*args`的使用。

`*args`是将一个迭代器里的所有内容拆分出来然后都作为参数传递给一个函数使用，比如这里，就是将strs内的所有的元素拆分以后全部给zip使用。虽然我记得`*args`大多是用在定义函数的参数上，用于不确定参数个数的时候，但是这个地方确实也是个非常好的应用，值得学习。

