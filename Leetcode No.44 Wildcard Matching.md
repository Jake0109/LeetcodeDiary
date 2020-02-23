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

在借鉴了RE题目中前人的智慧后，终于AC了...之后的明天继续。