# Leetcode Diary 10

# Description

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

## My solution

那么首先我们来看一下，这个问题涉及的问题就涉及'.'和'*'这两个符号。

总共有四种情况：

- 没有'.'，没有'*'
- 有'.'，没有'*'
- 没有'.'有'*'
- 有'.'，有'*'

最简单的第一种情况，最简单，直接 `s == p`这个条件就可以得出结果了。

第二种情况，因为s和p的长度相同，所以遍历整个str，当`p[i] == '.'`的时候，continue这个循环就可以了。

第三种情况，因为s和p的长度不同，所以需要一些别的方法来同时遍历两个str，并且来判断，是否匹配。

第四种情况可以整合到第三种情况里。

问题的分解已经结束了，那么就开始coding吧。

### 第一种情况

```python
def isMatch(s,p):
    if '.' not in p and '*' not in p:
        return s == p
```

### 第二种情况

```python
def isMatch(s,p):   
    for i in range(len(s)):
        if s[i] != p[i] and p[i] != '.':
            return False
        else:
            return True
```

### 第三种情况

这里选择了反向遍历的方法，~~其实正向也行~~虽然逻辑不是很乱，但是确实花了一点时间。

```python
def isMatch(s,p):
    j = len(s)-1
    for i in range(len(p)-1,-1,-1): #len(p) == len(arr)
        if p[i] == '*':
            continue
        elif i != len(p)-1 and p[i+1] == '*':
            while p[i] == s[j]:
                j -= 1
        else:
            if p[i] != s[j]:
                return False
            j -= 1
    else:
        return True
```

### 综合起来

这个时候需要改变的是“相等的条件”。

以前的相等是`s[j] == p[i]`不等是`s[j] !=  p[i]`。

现在的相等是`s[j] == p[i] or p[i] == '.'`不等是`s[j] != p[i] and p[i] != '.'`

整合出来就是这样的：

```python
def isMatch(s,p):
    if '.' not in p and '*' not in p:
        return s == p
    j = len(s)-1
    for i in range(len(p)-1,-1,-1): #len(p) == len(arr)
        if p[i] == '*':
            continue
        elif i != len(p)-1 and p[i+1] == '*':
            while p[i] == s[j] or p[i] == '.':
                j -= 1
        else:
            if p[i] != s[j] and p[i] != '.':
                return False
            j -= 1
    else:
        return True
```

上述方法看似解决了所有问题，但是会出现新的问题：当`.*`这个组合出现在字符串中间位置的时候，会发生在遍历过程中`.*`把当前位置之前（之后）所有字母都单个匹配了，而最后会报超出index范围的错。

那么`.*`出现的情况，就又需要单独讨论了。并且，出现了当输入s='aa' p='a*'的时候的超出索引范围的错误，原因在于s[-1,-2] == 'a' 所以会一直匹配下去，这么看来，索引中有负数，这个问题也需要好好思考一番。

### 修改

目前能想到的，除了出现`.*`的情况，不会出问题的solution是这样的。

```python
def ismatch(s,p):
    if '.' not in p and '*' not in p:
        return s == p
    j = len(s)-1
    for i in range(len(p)-1,-1,-1): #len(p) == len(arr)
        if p[i] == '*':
            continue
        elif i != len(p)-1 and p[i+1] == '*':
            while p[i] == s[j] and j >=0:
                j -= 1
        else:
            if p[i] != s[j] and p[i] != '.':
                return False
            j -= 1
    else:
        if j <= 0:
            return True
        else:
            return False
```

那么，当`.*`出现的情况，可以用上面的函数优先匹配非`.*`的部分。

```python
def isMatch(s,p):
    if '.*' in s:
        strs = s.split('.*')
        i = 0
        for st in strs:
            if st not in s[i::]:
                return False
            else:
                for j in range(i,len(s)):
                    if s[i] == str[0]:
                        i = j+len(str)
    else:
        return ismatch(s,p)
```

这个是目前想到的办法，但是仍然有问题，就是如果`.*`之外还有别的`.`或`*`出现的话，还是不能正确返回。~~正则表达式真难~~

> 博主败了,真的想不到办法来cover所有的情况了。

### Official Solution

#### Approach 1: Recursion

**Intuition**

If there were no Kleene stars (the `*` wildcard character for regular expressions), the problem would be easier - we simply check from left to right if each character of the text matches the pattern.

When a star is present, we may need to check many different suffixes of the text and see if they match the rest of the pattern. A recursive solution is a straightforward way to represent this relationship.

**Algorithm**

Without a Kleene star, our solution would look like this:

```python
def match(text, pattern):
    if not pattern: return not text
    first_match = bool(text) and pattern[0] in {text[0], '.'}
    return first_match and match(text[1:], pattern[1:])
```

> 这个我是能实现的，但是确实没有这么简洁。

If a star is present in the pattern, it will be in the second position \text{pattern[1]}pattern[1]. Then, we may ignore this part of the pattern, or delete a matching character in the text. If we have a match on the remaining strings after any of these operations, then the initial inputs matched.

```python
class Solution(object):
    def isMatch(self, text, pattern):
        if not pattern:
            return not text

        first_match = bool(text) and pattern[0] in {text[0], '.'}

        if len(pattern) >= 2 and pattern[1] == '*':
            return (self.isMatch(text, pattern[2:]) or
                    first_match and self.isMatch(text[1:], pattern))
        else:
            return first_match and self.isMatch(text[1:], pattern[1:])
```

> 如果说不带星号的solution我能说“学到了”，那么这个真的只能说叹为观止，自己的功夫还没练到家，原来还可以这么写。
>
> 之前满脑子都是循环的解决方案，没想到迭代竟能如此的简单高效。