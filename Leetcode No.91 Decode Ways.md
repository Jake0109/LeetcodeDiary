# Leetcode No.91 Decode Ways

## Description

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

**Example 1:**

```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

## My Solution

```python
class Solution:
    def numDecodings(self, s: str) -> int:

        dp = [1, ]

        if len(s) == 1:
            return 1

        dp.append(2 if int(s[0]) < 3 else 1)
        if len(s) == 2:
            return dp[1]

        for i in range(2,len(s)):
            dp.append(dp[i-1])

            if int(s[i-1:i+1]) <= 26 and i > 1:
                dp[i] = dp[i] + dp[i-2]

        return dp[-1]
```

这个是我最先的预想，因为从dp的角度来看，不过是根据前面的结果推得当下的结果，也就是一个费波纳提数列的变形罢了，但是这样的话容易忽略一个问题，就是"0"这个数字出现在`s`中的话会对这个结果产生什么样的影响。

当`0`出现在第一个字符，那么整个字符串应该都是不可达的，也就是返回值应为0。当在后续的遍历过程中出现`0`，假设`0`之前的数字为1或2，那么`0`所在位置的dp的值应该是`0`所在位置前一位的值；假设`0`之前的数字大于2，则整个结果应该返回0。

由此出发，开始对代码进行整改：

```python
class Solution:
    def numDecodings(self, s: str) -> int:

        if s.startswith("0"):
            return 0

        dp = [1, ]

        if len(s) == 1:
            return 1

        for i in range(1, len(s)):
			
            # when s[i] == "0", the process
            if s[i] == "0":
                if s[i-1] == "1" or s[i-1] == "2":
                    dp.append(dp[i-2])
                    continue
                else:
                    return 0

            if int(s[i-1:i+1]) <= 26 and s[i-1] != "0":
                dp.append(dp[i-1] + dp[i-2])
            else:
                dp.append(dp[i-1])

        # print(dp)
        return dp[-1]
```

## Reflection & Polishing-up

对于上述的解决方案，一方面去除了当s的长度为2的时候的特殊情况，因为它从当前的情况来看并不具备特殊性，那么由此衍生出来看，其实在上述方案中s长度为1的情况其实也没有了特殊性，因为当s长度为1，则不会进行遍历的操作。那么代码可以继续精简：

```python
class Solution:
    def numDecodings(self, s: str) -> int:

        if s.startswith("0"):
            return 0

        dp = [1, ]

        if len(s) == 1:
            return 1

        for i in range(1, len(s)):

            if s[i] == "0":
                if s[i-1] == "1" or s[i-1] == "2":
                    dp.append(dp[i-2])
                    continue
                else:
                    return 0

            if int(s[i-1:i+1]) <= 26 and s[i-1] != "0":
                dp.append(dp[i-1] + dp[i-2])
            else:
                dp.append(dp[i-1])

        # print(dp)
        return dp[-1]
```

这道题理论上是可以用递归来实现的，但是正如之前所说，关于`0`的问题，使得递归的条件判断会变得非常繁琐，所以不推荐。

## Conclusion

- 我终于把毕设做完了...虽然挺烂的，但是做完了。
- 在为人处世方面，还是要多多注意一点啊。