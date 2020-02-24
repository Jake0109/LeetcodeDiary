# Leetcode No.44 Wildcard Matching

## Description

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `?` or `*`.

**Example 1:**

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```

**Example 3:**

```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**

```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**

```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

## My Solution

```python
class Solution:
    def isMatch(self, string: str, pattern: str):
        if not pattern:
            return not string

        firstMatch = bool(string) and pattern[0] in {string[0], '?', '*'}

        if pattern[0] == '*':
            return (
                self.isMatch(string, pattern[1:]) or firstMatch and self.isMatch(string[1:], pattern)
            )
        else:
            return firstMatch and self.isMatch(string[1:], pattern[1:])
```

我觉得这样差不多可以了，然而，被这样一个测试用例打败了。

```python
p = "aaabbbaabaaaaababaabaaabbabbbbbbbbaabababbabbbaaaaba"
s = "a*******b"
```

超时，又是这种极其不合理的操作导致的...还挺有意思...然而就算我试着加一个减少连续'*'的过程在里面，最后还是失败了，因为，还有个更恐怖的：

```python
p = "babaaababaabababbbbbbaabaabbabababbaababbaaabbbaaab"
s = "***bba**a*bbba**aab**b"
```

好吧，看样子这个递归可能不太行。换DP试试：

```python
class Solution:
    def isMatch(self, string: str, pattern: str):
        cache = {}

        # in case that multiple * gathered together and make trouble
        while "**" in pattern:
            pattern = pattern.replace("**", "*")

        def dp(string_index, pattern_index):
            if (string_index, pattern_index) not in cache:
                if pattern_index == len(pattern):
                    ans = string_index == len(string)
                else:
                    first_match = string_index < len(string) and pattern[pattern_index] in {string[string_index], '?'}
                    if pattern[pattern_index] == '*':
                        ans = dp(string_index, pattern_index + 1) or string_index < len(string) and dp(string_index + 1, pattern_index)
                    else:
                        ans = first_match and dp(string_index + 1, pattern_index + 1)

                cache[string_index, pattern_index] = ans
            return cache[string_index, pattern_index]

        return dp(0, 0)
```

在借鉴了RE题目中前人的智慧后，终于AC了...

## Reflection & Polishing-up

这次Leetcode有点奇怪，参数的名命这么随意搞得讨论区里面也没看到几个命名规范的，看的着实头疼，RE里面的变量也都是`text, pattern`，这里都成了什么`s,p`，虽然这么说，该看的我也看了，绝大多数也都是O(M*N)的算法，不如自己再参考参考之前的写一遍自下而上的DP。

```python
class Solution:
    def wcMatch(self, text, pattern):
        # a len(pattern)+1 * len(text)+1 matrix
        # dp[-1][-1] is the initial statue (no element been mathed yet)
        dp = [[False] * (len(pattern) + 1) for _ in range(len(text) + 1)]
        dp[-1][-1] = True

        # just like top-bottom variation
        for text_index in range(len(text), -1, -1):
            for pattern_index in range(len(pattern) - 1, -1, -1):
                first_match = text_index < len(text) and pattern[pattern_index] in {text[text_index], '?'}
                if pattern[pattern_index] == '*':
                    dp[text_index][pattern_index] = dp[text_index][pattern_index + 1] or text_index < len(text) and dp[text_index + 1][pattern_index]
                else:
                    dp[text_index][pattern_index] = first_match and dp[text_index + 1][pattern_index + 1]

        return dp[0][0]
```

刚好借着这个机会再看看两个代码的性能差距，顺带再撸一个装饰器试试。

```python
import time


def time_counter(func):
    def wrapper(*args, **kwargs):
        tip = time.perf_counter()
        res = func(*args, **kwargs)
        top = time.perf_counter()
        print("func {} runtime:{}".format(func, top - tip))
        return res

    return wrapper

#input
sol = Solution()
s = "leetcode"
p = "*e*t?d*"
print(sol.isMatch(s, p), sol.wcMatch(s, p))

# output
func <function Solution.isMatch at 0x0000029F52FE58B8> runtime:3.189999999999443e-05
func <function Solution.wcMatch at 0x0000029F52FE5AF8> runtime:4.0400000000002934e-05
False False
```

完工~但其实我还是不太明白为什么自下而上和自上而下的DP有这么大的性能差别...我知道自顶向下占用的内存肯定更少，但是时间方面，暂时还没想出一个比较合理的解释。

## Conclusion

- 学而时习之，不亦乐乎。多多复习，切勿捡了芝麻丢了西瓜。
- 命名规范很重要。命名规范很重要。命名规范很重要。并不是因为说前段时间刚看完规范相关的东西，确实，写的规范的代码和不规范的代码，即使功能和实现相同，阅读的难度，以及阅读的时间差距很大。