# Leetcode No.22 Generate Parentheses

## Description

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```     
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

## My Solution

```python
def generateParenthesis(n: int):
    if n == 1:
        return ["()"]
    elif n == 0:
        return [""]
    else:
        s = set()
        for i in generateParenthesis(n-1):
            s.add("("+i+")")
            s.add("()"+i)
            s.add(i+"()")
        return list(s)
```

这个是我通过n=1,n=2,n=3推出的一个递归的算法，但是在n=4的时候，上述算法少了一条路径，也就是少了一种情况`(())(())`。重新想吧...

```python
class Solution:
    def generateParenthesis(self,n: int):
        if n == 1:
            return ["()"]
        elif n == 0:
            return [""]
        else:
            s = set()
            for i in self.generateParenthesis(n-1):
                s.add("("+i+")")
            for i in range(1,n):
                for j in self.generateParenthesis(i):
                    for k in self.generateParenthesis(n-i):
                        s.add(j+k)
            return list(s)
```

那么这样就完成了一个用迭代思路而延伸出来的solution。之前我还是不太会写迭代来着，也算看到了一点自己的进步。

## Reflection & Polishing-up

在一次次的迭代的算法中，可能会有多次去进行相同操作的情况存在，比如当n=3时：

```python
        else:
            s = set()
            for i in self.generateParenthesis(n-1):
                s.add("("+i+")")
            for i in range(1,n):
                for j in self.generateParenthesis(i):
                    for k in self.generateParenthesis(n-i):
                        s.add(j+k)
```

来看一下这一段代码块，首先去进行一次`for i in self.generateParenthesis(n-1):`执行一次n=2的函数，然后，又会在下面的代码块中：

```python
            for i in range(1,n):
                for j in self.generateParenthesis(i):
                    for k in self.generateParenthesis(n-i):
                        s.add(j+k)
```

分别再执行n=1,n=2;n=2,n=1的函数。

相同参数的函数执行并不会留下缓存的结果，这样会延长程序的运行时间。

这里，博主想到的是，用dictionary做一个cache。

```python
class Solution:
    def generateParenthesis(self,n: int):
        cache = {0: [""], 1: ["()"]}
        if n in [0,1]:
            return cache[n]
        else:
            for i1 in range(2,n+1):
                tset = set()
                for  j in cache[i1-1]:
                    tset.add("("+j+")")
                for i2 in range(1,i1):
                    for j in cache[i2]:
                        for k in cache[i1-i2]:
                            tset.add(j+k)
                cache[i1] = list(tset)
        return cache[n]
```

果不其然，这个solution明显快了很多。

| Time Submitted    | Status                                                       | Runtime | Memory  | Language |
| :---------------- | :----------------------------------------------------------- | :------ | :------ | :------- |
| a few seconds ago | [Accepted](https://leetcode.com/submissions/detail/297767262/) | 20 ms   | 12.9 MB | python3  |
| 18 minutes ago    | [Accepted](https://leetcode.com/submissions/detail/297762529/) | 36 ms   | 12.9 MB | python3  |

但是让我不解的地方是，在这个运用了cache的solution中，我明显是使用了用空间换时间的策略，但是Memory的使用两者确是相同的，这个我暂时没有想出来为什么。

现在就去看看别人是怎么写的吧。首先是Official Solution的这个：

#### Approach 3: Closure Number

**Intuition**

To enumerate something, generally we would like to express it as a sum of disjoint subsets that are easier to count.

Consider the *closure number* of a valid parentheses sequence `S`: the least `index >= 0` so that `S[0], S[1], ..., S[2*index+1]` is valid. Clearly, every parentheses sequence has a unique *closure number*. We can try to enumerate them individually.

**Algorithm**

For each closure number `c`, we know the starting and ending brackets must be at index `0` and `2*c + 1`. Then, the `2*c` elements between must be a valid sequence, plus the rest of the elements must be a valid sequence.

```python
class Solution(object):
    def generateParenthesis(self, N):
        if N == 0: return ['']
        ans = []
        for c in xrange(N):
            for left in self.generateParenthesis(c):
                for right in self.generateParenthesis(N-1-c):
                    ans.append('({}){}'.format(left, right))
        return ans
```

跟我的一段代码是完成的同一个功能，然而我甚至需要用到set，这个就非常的简洁，代码可读性很高。当然，我觉得我运用cache的思路应该也是能大幅提高运行效率的。

