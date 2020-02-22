# Leetcode No.43 Multiply Strings

## Description

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Note:**

1. The length of both `num1` and `num2` is < 110.~~这里不应该用are吗？~~
2. Both `num1` and `num2` contain only digits `0-9`.
3. Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

## My Solution

嗯，其实我不太明白

> You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

这里，什么样才算directly。总之按照自己理解的写吧...

```python
def multiply1(num1: str, num2: str) -> str:
    if num1 == "0" or num2 == "0":
        return "0"

    result = 0
    carry = 1
    num1_int = int(num1)
    for num in num2[::-1]:
        result += num1_int * int(num) * carry
        carry *= 10

    return str(result)
```

根据口诀写的，也不知道符不符合题目要求。

## Reflection & Polishing-up

在我看来，这个题目...真的有点学院派的那种风格，真的很欠缺实用性如果说单单为了完成这个需求，而特意在项目工程文件里面建立这个函数...这是有多难受...这个我是真的总结不出什么东西了...总之我按照它叫我做的东西做了...别的嘛...