# Leetcode 日记

一时兴起，开始进行这项活动，一个是开始慢慢做leetcode的题目，另外就是，把一些思路和想法记录下来，以后可以稍微参考一下。

## Longest palindrome substring

最大的回文子字符串。

熟悉博主的都知道，博主是个小白啊，回文字符串，这个都没做过，所以那么我们可以简单的把这个题目拆分一下：

```python
def longestPalindrome(s):
	#对字符串的字符两两对应
    for i in range(len(s)):
        for j in range(len(s)+1,i,-1):
            #判断是否回文
    return res
```

那么怎么判断一个字符串是否是回文字符串呢。博主一开始的解决方案是这个：

```python
def isPalindrome(s):
    for i in range(len(s)):
        if s[i] != s[len(s)-i-1]
        return False
    else:
        return True
```

这里用了python里面一个特别的else使用方法：for...else...指的是，当for循环执行结束，如果没有return等特殊状况就结束的话，就会执行else里面的代码。

此时代码就可以完成需求了：寻找字符串的最大回文子字符串

```python
def isPalindrome(s):
    for i in range(len(s)):
        if s[i] != s[len(s)-i-1]
        return False
    else:
        return True

def longestPalindrome(s):
    res = ""
    length = 0
    for i in range(len(s)):
        for j in range(len(s)+1,i,-1):
            if isPalindrome(s[i:j]) == True:
                if j-i+1>length:
                    res = s[i:j]
                    length = j-i+1
     return res
```

这个就是最暴力的解法，时间复杂度为O(n^3)可以说效率非常低了。那么我们一步步慢慢看。

### 优化回文字符串的判断

在python中，分片是个非常好用的功能。比如上面的判断回文字符串可以用分片来解决

```python
#首先可以把判断的条件更换成这样,因为python中支持负数索引
s[i] != s[-i-1]
#但是这样，回文字符串的判断时间复杂度依然是O(n)
#用了分片以后就可以这么判断
def isPalindrome(s):
    if s == s[::-1]:
        return True
    else:
        return False
```

这样的话，时间复杂度是O(1)了，那么当longestPalindrome()调用isPalindrome()的时候，整体的时间复杂度就是O(n^2)可以说是一个挺高的优化了。

## 使用DP思维

动态规划的思维来分析一下这个问题。

对于任意一个子字符串s[i:j+1]，它是一个回文子字符串的充分条件是什么？

```bash
palindrome(s[i:j+1]) = 	s[i]==s[j],								j-i<=1
						s[i]==s[j]&palindrome(s[i+1:j]),		j-i>1
```

那么我们开始coding

```python
def longestPalindrome(s):
    l = len(s)
    matrix = [[0 for i in range(l)] for i in range(l)]
    longestStr = ""
    longestLen = 0

    for j in range(l):
        for i in range(0,j+1):
            if j - i <= 1:
                if s[i] == s[j]:
                    matrix[i][j] = 1
                    if j-i+1 > longestLen:
                        longestLen = j-i+1
                        longestStr = s[i:j+1]
            else:
                if s[i] == s[j] and matrix[i+1][j-1] == 1:
                    print(s[i:j+1])
                    matrix[i][j] = 1
                    if j-i+1 > longestLen:
                        longestLen = j-i+1
                        longestStr = s[i:j+1]
    return longestStr
```

需要注意的是，在这个二重循环的部分，因为我是用一个matrix来保存是否是palindrome的状态，而，对于这个矩阵来说，有用的只有右上部分的矩阵，而且对于matrix[1,3]依赖matrix[2,2]，所以如果不想多次扫描这个矩阵的话，可以用我列出来的这个遍历的方法。

虽然提交的时候，下面这个DP的方法也只比34%的人快，但是，对我来说，这个已经是比较高级的算法了。~~想要再深入的话，我们后面再说。~~