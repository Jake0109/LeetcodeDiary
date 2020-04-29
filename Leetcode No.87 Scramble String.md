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

这样的话，我觉得就能完成目标了，但是，时间复杂度过高，会超时。我明天再看看。