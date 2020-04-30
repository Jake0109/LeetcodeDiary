# Leetcode No.87 Scramble String

## Description

Given a string *s1*, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of *s1* = `"great"`:

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node `"gr"` and swap its two children, it produces a scrambled string `"rgeat"`.

```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

We say that `"rgeat"` is a scrambled string of `"great"`.

Similarly, if we continue to swap the children of nodes `"eat"` and `"at"`, it produces a scrambled string `"rgtae"`.

```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```

We say that `"rgtae"` is a scrambled string of `"great"`.

Given two strings *s1* and *s2* of the same length, determine if *s2* is a scrambled string of *s1*.

**Example 1:**

```
Input: s1 = "great", s2 = "rgeat"
Output: true
```

**Example 2:**

```
Input: s1 = "abcde", s2 = "caebd"
Output: false
```

## My Solution

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        def rec(s1, s2):
            # print(f"s1:{s1} s2:{s2}")
            if len(s1) == 1:
                return s1 == s2

            mid = len(s1) // 2

            if len(s1)%2:
                situation1 = (rec(s1[:mid], s2[:mid]) and rec(s1[mid:], s2[mid:])) or (rec(s1[:mid+1], s2[:mid+1]) and rec(s1[mid+1:], s2[mid+1:]))
                situation2 = (rec(s1[:mid], s2[mid+1:]) and rec(s1[mid:], s2[:mid+1])) or (rec(s1[:mid+1], s2[mid:]) and rec(s1[mid+1:], s2[:mid]))
            else:
                situation1 = (rec(s1[:mid], s2[:mid]) and rec(s1[mid:], s2[mid:]))
                situation2 = rec(s1[:mid], s2[mid:]) and rec(s1[mid:], s2[:mid])

            # print(situation1 or situation2)
            return situation1 or situation2

        return rec(s1, s2)
```

这是个错误的Solution，错误原因在于我没有认清一点：将字符串分为两个非空字符串并存入二叉树的两个节点，这个过程不一定是最优的。也就是说，如果字符串长度为4，那不一定是分为两个长度为2的子字符串。

其实这样的话，就需要改了：

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        def rec(s1, s2):
            print(f"s1:{s1} s2:{s2}")
            length = len(s1)
            if length <= 2:
                return s1 == s2 or s1 == s2[::-1]

            for i in range(1,length):
                print(i)
                ans = rec(s1[:i],s2[:i]) and rec(s1[i:],s2[i:]) or rec(s1[:i],s2[length-i:]) and rec(s1[i:],s2[:length-i])
                if ans:
                    return True
            return False

        return rec(s1, s2)
```

这样的话，我觉得就能完成目标了，但是，时间复杂度过高，会超时。参考了一下别人的算法：

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        def rec(s1, s2):
            # if there is letters in s1 and not in s2 return False
            t1 = sorted(s1)
            t2 = sorted(s2)
            if t1 != t2:
                return False
            
            length = len(s1)
            if length <= 2:
                return s1 == s2 or s1 == s2[::-1]

            for i in range(1,length):
                ans = rec(s1[:i],s2[:i]) and rec(s1[i:],s2[i:]) or rec(s1[:i],s2[length-i:]) and rec(s1[i:],s2[:length-i])
                if ans:
                    return True
            return False

        return rec(s1, s2)
```

添加了这么一个步骤以后，就可以避免很多不必要的步骤，效率也可以高很多。

## Reflection & Polishing-up

这部分明天再写吧，我看到了用DP的思路了，但是今天光是想递归的算法就已经头疼脑袋大了。

评论区里面真的都是人才啊，稍微看了几个就真的从内心深处这么觉得了，先看一个DP的：

```python
class Solution:
    def isScramble(self, s1: str, s2: str) -> bool:
        if len(s1)!=len(s2):
            return False
        if s1==s2:
            return True
        dp=[[[False]*(len(s1)+1)  for j in range(len(s1))] for i in range(len(s1))]
        for k in range(1,len(s1)+1):
            for i in range(len(s1)):
                for j in range(len(s1)):
                    if k==1:
                        dp[i][j][k]=s1[i]==s2[j]
                        continue
                    for h in range(1,k):
                        left=(dp[i][j][h] and dp[i+h][j+h][k-h]) if j+h<len(s1) and i+h<len(s1) else False
                        right=(dp[i][j+k-h][h] and dp[i+h][j][k-h]) if j+k-h<len(s1) and i+h<len(s1) else False
                        dp[i][j][k]=dp[i][j][k] or left or right
        return dp[0][0][len(s1)]
```

说实话，人看傻了...三维矩阵，这个DP可能更好理解一点：

```python
def isScramble(self, s1, s2):
    dp = {}
    
    def helper(s1, s2):
        if (s1, s2) in dp:
            return dp[(s1, s2)]
        if len(s1)==1:
            return s1==s2
        if len(s1) != len(s2) or sorted(s1)!= sorted(s2):
            return False
        for i in range (1,len(s1)):
            if helper(s1[:i], s2[:i]) and helper(s1[i:] , s2[i:]) or helper(s1[:i], s2[-i:]) and helper(s1[i:], s2[:-i]):
                dp[(s1,s2)] = True
                return True
        dp[(s1,s2)] = False
        return False
    
    return helper(s1, s2)
```

但说是DP，其实本质上是带memo的递归，所以可能纯的DP的话，还需要看上面那个。

然后就是一个三行代码的递归Solution：

```python
def isScramble(self, s1: str, s2: str) -> bool:
        if s1==s2:return 1
        if sorted(s1)!=sorted(s2): return 0
        return any(self.isScramble(s1[:i],s2[:i]) and self.isScramble(s1[i:],s2[i:]) or
                   self.isScramble(s1[:i],s2[-i:]) and self.isScramble(s1[i:],s2[:-i]) 
                   for i in range(1,len(s1)))
```

嗯...真的是人才，any()函数用的非常到位，之前我没想到。

## Conclusion

- 学到的一点是，在Python的切片操作中，可以使用list[-i:]这样的操作。
- 以后如果再遇到递归的题目，可以试试直接递归题目给的函数。