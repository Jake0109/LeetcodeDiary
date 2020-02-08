# Leetcode No.32 Longest Valid Parentheses

## Description

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

**Example 2:**

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

## My Solution

```python
class Solution:
    def longestValidParentheses(self,s: str) -> int:
        res = 0
        if not s or len(s) == 1:
            return res

        for i in range(0,len(s)-1):
            for j in range(len(s)-1,i,-1):
                if j-i+1 < res:
                    break
                stack = []
                for p in s[i:j+1]:
                    if p == '(':
                        stack.append(p)
                    else:
                        if not stack:
                            break
                        stack.pop()
                else:
                    if not stack:
                        res = j - i + 1
        return res
```

我觉得从解法上来说应该是没问题的，但是O(n^3)让它超时了...苦思冥想了半天，也没找到个能O(n^2)的Solution，就先去官方的Solution里面看一下。

？？？

```java
public class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push('(');
            } else if (!stack.empty() && stack.peek() == '(') {
                stack.pop();
            } else {
                return false;
            }
        }
        return stack.empty();
    }
    public int longestValidParentheses(String s) {
        int maxlen = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 2; j <= s.length(); j+=2) {
                if (isValid(s.substring(i, j))) {
                    maxlen = Math.max(maxlen, j - i);
                }
            }
        }
        return maxlen;
    }
}
```

官方的暴力解法是这样的，我寻思着，我们两个也没什么区别啊，为什么你到solution里面了，我就只能超时呢？算了，对于一个提升自我能力为驱动的博主来说，这无所谓。~~虽然这证明了，博主能力还是有的！~~

我们继续看别的Solution，这个DP解决的：

> https://leetcode.com/problems/longest-valid-parentheses/solution/ 我把链接放在这了，整个solution的篇幅太大，放进来不合适。

看完基本就知道思路了，我们自己来实现一下。

```python
def longestValidParentheses(s: str) -> int:
    if not s:
        return 0
    dp = list()
    for i in range(len(s)):
        if s[i] == '(':
            dp.append(0)
        else:
            if i == 0:
                dp.append(0)
            elif s[i-1] == '(':
                dp.append(dp[i-2] + 2)
            else:
                if i - dp[i-1] - 1 >= 0 and s[i-dp[i-1]-1] == '(':
                    dp.append(dp[i-1] + dp[i-dp[i-1]-2] + 2 if i-dp[i-1]-2 >= 0 else dp[i-1] + 2)
                else:
                    dp.append(0)
    return max(dp)
```

涂涂改改以后，差不多就是这个样子AC的。这个时候就会想到python的切片有利有弊，比起java这类的语言，在s[-1]有的时候会捣乱的。

## Reflection & Polishing-up

我来试试把别的几个Approach也做了，No.3 Approach Using Stack：

```python
class Solution:
    def longestValidParentheses(self,s: str) -> int:
        maxlen = 0
        stack = [-1,]
        for i,p in enumerate(s):
            if p == '(':
                stack.append(i)
            else:
                stack.pop()
                if not stack:
                    stack.append(i)
                else:
                    maxlen = max(maxlen,i-stack[-1])
        return maxlen
```

这个是真的非常的巧妙，没想到stack还能这么用，原来以为能用上stack的我已经够厉害了...也就是说同样是用stack的Approach，我是O(n^3)人家是O(n)~~你怎么这么菜~~

## Conclusion

- 能够想到使用stack，是一个非常好的开始。
- stack的使用不熟练，能够用更短的时间，还有提升空间。
- DP的思路理不清楚，以后加强逻辑训练。