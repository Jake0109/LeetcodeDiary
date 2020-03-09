# Leetcode No.58 Length of  Last Word

## Description

Given a string *s* consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

**Note:** A word is defined as a **maximal substring** consisting of non-space characters only.

**Example:**

```
Input: "Hello World"
Output: 5
```

## My Solution

先放一个我第一反应写下来的一行代码

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.strip().split(' ')[-1])
```

~~其实我一开始忘了strip()~~因为我没想到最后一个字符也可以是空格...

然后写一个稍微正常一点的。

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        index = len(s) - 1
        length = 0
        for i in range(index, -1, -1):
            if s[i] == " " and not length:
                continue
            elif s[i] == " ":
                break
            else:
                length += 1

        return length
```

也就行了，简单的题目，没什么好说的。

## Reflection & Polishing-up

一点好奇心驱使我想去考察一下内置函数的实现和自己实现的runtime对比：

```python
str = "asdf  asd fasdfas f asdf asdfasdfda sfsafddfasfdf sdf asf asdfsadf as  sdfas dfasf asdf asd  asdfas dfas as  fasdfasfd safsadf a sdfd"
print(sol.lengthOfLastWord(str))
print(sol.lengthOfLastWord1(str))
```

写了一个装饰器，之前的篇目中已经写了很多次的了。

```python
func:lengthOfLastWord takes 3.900000000001125e-06
4
func:lengthOfLastWord1 takes 5.699999999997374e-06
4
```

得到的结果是这样的，也就是内置的函数效率还是大于我们自己写的

## Conclusion

- 有点想不出来能总结出什么，但是我觉得我之前的一个观点能在这运用一下：
  - 要坚持自己手写一些基本的代码。但是也需要知道当前的编程语言给自己提供了什么便捷的方式。
- 再者的话，可能一些过于底层的东西不一定，因为去看一些太过基础的东西，源码里面只有 pass 。但是学架构的时候尽量的去看看源码，我觉得对个人对架构的深入理解是非常有帮助的。