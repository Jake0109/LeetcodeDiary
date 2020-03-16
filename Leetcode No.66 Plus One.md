# Leetcode No.66 Plus One

## Description

Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

**Example 1:**

```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

**Example 2:**

```
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```

## My Solution

```python
class Solution:
    def plusOne(self, digits):
        res = digits[::]

        if res[-1] != 9:
            res[-1] += 1
        else:
            for i in range(len(digits)-1, -1, -1):
                if digits[i] == 9:
                    res[i] = 0
                    continue
                else:
                    res[i] += 1
                    break
            else:
                res = [1] + [0 for i in range(len(digits))]

        return res
```

## Reflection & Polishing-up

挺简单的一道题目，我个人觉得，肯定有人会这么写：

```python
class Solution:
    def plusOne(self, digits):
        num = 0
        for digit in digits:
            num *= 10
            num += digit
        num += 1
        res = []
        while num:
            res.append(num % 10)
            num //= 10
        res.reverse()
        return res
```

发现：现在的这个方法比之前的方法快很多，稍微有点不解，于是自己拿了几组数据测试了一下：

```
input digits = [8,9,9]
plusOne takes 1.2900000000003187e-05 sec
plusOneOrigin takes 6.699999999998374e-06 sec

input digits = [8,8,1]
plusOne takes 1.2400000000002687e-05 sec
plusOneOrigin takes 4.900000000002125e-06 sec
```

我觉得这两个示例已经可以说明情况了，无论是在需要多次进位还是不用进位的情况下，新版的plusOne都比之前的原版要更快，说实话，我有点想不清为什么。在leetcode多次提交了相同的代码以后，也出现了runtime不同的情况出现，挺苦恼的...

## Conclusion

- 最想反思的是昨天的一点：
  - 其实到最后我的代码已经是将各个可能的情况都拆分开来用正则表达式来做了，但是确实没有算计到"\d+\.e\d+"这样的算法，实际上，如果加上的话，应该这题就能解开了。
  - 有一点学校方面事情繁杂的焦躁，带上一点不冷静导致这题没有AC，以后要反思。