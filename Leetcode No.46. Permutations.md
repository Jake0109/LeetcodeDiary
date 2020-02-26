# Leetcode No.46. Permutations

## Description

Given a collection of **distinct** integers, return all possible permutations.

**Example:**

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## My Solution

```python
class Solution:
    def permute(self, nums):
        res = []
        temp_perm = []

        def iterate(index=0):
            for i in nums:
                if i in temp_perm:
                    continue
                temp_perm.append(i)
                index += 1
                iterate(index=index)
                if index == len(nums):
                    res.append(temp_perm.copy())
                
                # backtrack
                temp_perm.remove(i)
                index -= 1

        iterate()
        return res
```

写了一个递归函数来实现的，思路有点像回溯。只不过回溯是search，我这个是contribute。

## Reflection & Polishing-up

去中文网看了一下，原来官方的解法就是回溯...所以我这个就是回溯...只能说，世界观不健全时单纯使用方法论的劣势就体现出来了。因为中文网里面有解法，所以总感觉那里讨论区里清一色的回溯，或者说深度优先，来国际版里面倒是看到了一些新意。

```python
def permute_recursion(self,nums):
    if not nums: return []

    result = []

    def perms(taken, left):

        # if nothing is left to pick, append what we've found so far to the result
        if len(left) == 0:
            result.append(taken)
            return

        # go through all options that are left and process them recursively
        for i in range(len(left)):
            temp = left[i]
            perms(taken + [temp], left[:i] + left[i + 1:])

            perms([], nums)
            return result
```

这个就是个纯的递归了，因为参数的设置跟回溯还是不太一样。不过很简洁，我个人比较喜欢。

## Conclusion

- 第一次在没有外部帮助的情况下，自己使用回溯算法，还是挺有成就感的。