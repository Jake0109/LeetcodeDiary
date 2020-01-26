# Leetcode 日记9 Palindrome Number

## Description

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

## My Solution

不说什么，No.5里面有过相同的逻辑。很简单。

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if str(x)[::-1] == str(x):
            return True
        else:
            return False
```

## polishing-up

one-line code.

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```

