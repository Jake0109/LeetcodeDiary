# Leetcode No.72 Edit Distance

## Description

Given two words *word1* and *word2*, find the minimum number of operations required to convert *word1* to *word2*.

You have the following 3 operations permitted on a word:

1. Insert a character
2. Delete a character
3. Replace a character

**Example 1:**

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

**Example 2:**

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

## My Solution

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m = len(word1)
        n = len(word2)

        dp = [[0 for _ in range(m+1)] for _ in range(n+1)]

        for i in range(1,m+1):
            dp[0][i] = i
        for i in range(1,n+1):
            dp[i][0] = i

        for i in range(n):
            for j in range(m):
                if word1[j] == word2[i]:
                    dp[i+1][j+1] = 1 + min(dp[i][j+1], dp[i+1][j], dp[i][j] - 1)
                else:
                    dp[i+1][j+1] = 1 + min(dp[i][j+1], dp[i+1][j], dp[i][j])

        return dp[-1][-1]
```

我先坦白，不是我自己想出来的，苦思冥想，又试了半个小时，都没想出来内在逻辑，所以直接去看给的Solution了。

`dp[i][j]`代表将`word1[:i+1]`变成`word2[:j+1]`需要的最少步骤数，如`word1 = "horse", word2 = "ros"`，其中`dp[3][2]`代表hors改变到ros的最小步骤数。

主要逻辑在这两步：

```python
if word1[j] == word2[i]:
    dp[i+1][j+1] = 1 + min(dp[i][j+1], dp[i+1][j], dp[i][j] - 1)
else:
    dp[i+1][j+1] = 1 + min(dp[i][j+1], dp[i+1][j], dp[i][j])
```

先讲通用的：

`dp[i+1][j+1] = 1 + dp[i][j+1]`：在末尾添加一个字母

`dp[i+1][j+1] = 1 + dp[i+1][j]`：在末尾去除一个字母

然后当末尾的字母相同的时候：

`dp[i+1][j+1] = dp[i][j+1]`：什么都不用做

不同的时候：

`dp[i+1][j+1] = 1 + dp[i][j]`：改变末尾的一个字母

## Reflection & Polishing-up

在理解了思路以后，递归和迭代其实就都无所谓了，比如就可以这样：

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        if not len(word1) or not len(word2):
            return len(word1) or len(word2)
        if word1[0] == word2[0]:
            return self.minDistance(word1[1:], word2[1:])
        insert = 1 + self.minDistance(word1, word2[1:])
        delete = 1 + self.minDistance(word1[1:], word2)
        replace = 1 + self.minDistance(word1[1:], word2[1:])
        return min(insert, delete, replace)
```

当然这不是我写的，但是总体思路是相同的，基本也没有太多什么出彩的解法了。

## Conclusion

- 众所周知，我写这个Leetcode笔记，包括我在慢慢的做leetcode题目，就是为了锻炼自己的算法能力，但是显然还不够。
- 感觉这个跟那种人脸识别，或是那种Vtuber类似的东西中的算法应该是类似的，应该是机器学习之类的算法的基础吧。
- 目前在一边完善毕业设计，一边看算法书，还在练字，稍微过一会还需要看看redis, kafka这些NoSQL和MQ的东西，还是挺累的。