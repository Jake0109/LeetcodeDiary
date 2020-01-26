# Leetcode No.19 Valid Parentheses

## Description

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

## My Solution

```python
def isValid(s: str) -> bool:
    l = list(s)
    def valid(i):
        if l[i]=="[" and l[i+1]=="]":
            return True
        elif l[i]=="(" and l[i+1]==")":
            return True
        elif l[i]=="{" and l[i+1]=="}":
            return True
        else:
            return False
    for j in range(len(l)//2):
        for i in range(len(l)):
            if valid(i):
                l.pop(i)
                l.pop(i)
                break
    if l:
        return False
    else:
        return True
```

一个挺笨的O(n^2)的方法，就是每次遍历的时候如果发现存在连续的{},(),[]存在的话，将它们pop出去，如果结果为True的情况，经过len(s)//2的次数后list为空，反之，list还存在元素。

## Reflection & Polishing-up

改进版:

```python
class Solution(object):
    def isValid(self, s):
        while "()" in s or "{}" in s or '[]' in s:
            s = s.replace("()", "").replace('{}', "").replace('[]', "")
        return s == ''
```

官方solution中，使用栈的版本：

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """

        # The stack to keep track of opening brackets.
        stack = []

        # Hash map for keeping track of mappings. This keeps the code very clean.
        # Also makes adding more types of parenthesis easier
        mapping = {")": "(", "}": "{", "]": "["}

        # For every bracket in the expression.
        for char in s:

            # If the character is an closing bracket
            if char in mapping:

                # Pop the topmost element from the stack, if it is non empty
                # Otherwise assign a dummy value of '#' to the top_element variable
                top_element = stack.pop() if stack else '#'

                # The mapping for the opening bracket in our hash and the top
                # element of the stack don't match, return False
                if mapping[char] != top_element:
                    return False
            else:
                # We have an opening bracket, simply push it onto the stack.
                stack.append(char)

        # In the end, if the stack is empty, then we have a valid expression.
        # The stack won't be empty for cases like ((()
        return not stack
```

没想到能用栈来解决...鉴于最近过年，感想就少写一点啦，博主不能随心所欲敲代码也很无奈~~其实很开心~~