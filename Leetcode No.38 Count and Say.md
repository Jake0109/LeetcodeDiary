# Leetcode No.38 Count and Say

## Description

The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2`, then `one 1"` or `1211`.

Given an integer *n* where 1 ≤ *n* ≤ 30, generate the *n*th term of the count-and-say sequence. You can do so recursively, in other words from the previous member read off the digits, counting the number of digits in groups of the same digit.

Note: Each term of the sequence of integers will be represented as a string.

## My Solution

```python
class Solution:
    def countAndSay(self, n: int) -> str:
        if n == 1:
            return "1"
        else:
            res = ""
            count = 1
            pre = None
            nums = self.countAndSay(n-1)
            for i, num in enumerate(nums):
                if i + 1 < len(nums) and num == nums[i+1]:
                    count += 1
                else:
                    res += "%d%s" % (count, num)
                    count = 1
            return res
```

挺简单的，没什么技术含量，就不在这吹了。这里当然，用list+join的方法应该也是可行的。

## Reflection & Polishing-up

```python
class Solution:
    def countAndSay(self, n: int) -> str:
        if n == 1:
            return "1"
        res = ""
        count = 1
        pre = None
        nums = self.countAndSay(n-1)
        for i, num in enumerate(nums):
            if i + 1 < len(nums) and num == nums[i+1]:
                count += 1
            else:
                res += f"{count}{num}"
                count = 1
        return res
```

在看python课程的时候，看到过这个f-string。应该是目前对于string中添加参数的情况下，最可读的解决方案了，尝试了一下。

另外说一点，这里不推荐用list + join的方法来写的原因是题目中明确说明了，n<=30，当len(str)<1M~~大概是这个数量~~的时候，直接 str1 += str2 是比 list + join 的方案要更快的，但是当数量上去以后，append/extend + join 的开销会更小，所以这里我直接用str来操作了。

## Conclusion

- 我尝试了用生成器来解决这个问题，但是想要在一个迭代实现的问题中用生成器，现在想想好像有点难。
- 有的时候else是否需要写，不需要的时候可以省略。