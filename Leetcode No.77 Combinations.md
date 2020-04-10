# Leetcode No.77 Combinations

## Description

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.

**Example:**

```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

## My Solution

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        
        res = []
        ans = []

        def backtrack():
            if len(ans) == k:
                res.append(ans.copy())
                return

            for item in range(1,n+1):
                if not ans or item > ans[-1]:
                    ans.append(item)
                    backtrack()
                    ans.remove(item)
        backtrack()
        return res
```

回溯法，不是什么难问题。

## Reflection & Polishing-up

另一种解法：字典序组合

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        # init first combination
        nums = list(range(1, k + 1)) + [n + 1]
        
        output, j = [], 0
        while j < k:
            # add current combination
            output.append(nums[:k])
            # increase first nums[j] by one
            # if nums[j] + 1 != nums[j + 1]
            j = 0
            while j < k and nums[j + 1] == nums[j] + 1:
                nums[j] = j + 1
                j += 1
            nums[j] += 1
            
        return output
```

这里给一个leetcode中文网的链接：https://leetcode-cn.com/problems/combinations/solution/zu-he-by-leetcode/这里有它的所有介绍。

但是光看一遍介绍再看一遍代码应该时看不懂的，多琢磨琢磨。确实是一个很不错的Solution。

## Conclusion

- 不知道为啥，最近没啥干劲，写完这个笔记，也就花个一个多小时看看书了，希望开学前能找点什么东西刺激一下，再好好学习一波。