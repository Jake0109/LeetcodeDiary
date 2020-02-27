# Leetcode No.47 Permutation II

## Description

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

**Example:**

```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## My Solution

```python
class Solution:
    def permuteUnique(self, nums):
        res = []
        temp_perm = []

        def backtrack(nums):
            if not nums and temp_perm not in res:
                res.append(temp_perm.copy())
                return
            for i, num in enumerate(nums):
                temp_perm.append(num)
                backtrack(nums[:i] + nums[i+1:])
                temp_perm.pop()
            return

        backtrack(nums)
        return res
```

首先是这个根据上一道题改的这个，自然效率是非常低的，因为都是全排列，而其中很多是要被丢弃的解。

## Reflection & Polishing-up

开始优化：

```python
class Solution:
    def permuteUnique(self, nums):
        res = []
        temp_perm = []

        def backtrack(nums):
            if not nums:
                res.append(temp_perm.copy())
                return
            for i, num in enumerate(nums):
                
                # if num has appeared before, skip
                # because I pass nums[:i] + nums[i+1:] as nums
                # so only when encountering the num used it skip. 
                if num in nums[:i]:
                    continue
                    
                temp_perm.append(num)
                backtrack(nums[:i] + nums[i+1:])
                temp_perm.pop()
            return

        backtrack(nums)
        return res
```

注释我已经写了，请自己看。

讨论区的套路，感觉都大差不差...也没什么好说的了...

虽然想说现在什么都没有了，但是突然想到了一个好玩的点：

- 假设，只能写出来一个有重复全排列的函数，能不能做这一题呢？

当然可以，我甚至为了它写了一个装饰器：

```python
def make_unique(func):
    def wrapper(*args, **kwargs):
        reiterated = func(*args, **kwargs)
        print(reiterated)
        i = 0
        while i < len(reiterated):
            if reiterated[i] in reiterated[:i] + reiterated[i + 1:]:
                reiterated.pop(i)
                continue
            i += 1
        return reiterated
    return wrapper
```

这个是我没有去重的未优化的全排列

```python
    @make_unique
    def permuteNotUnique(self, nums):
        res = []
        temp_perm = []

        def backtrack(nums):
            if not nums:
                res.append(temp_perm.copy())
                return
            for i, num in enumerate(nums):
                temp_perm.append(num)
                backtrack(nums[:i] + nums[i+1:])
                temp_perm.pop()
            return

        backtrack(nums)
        return res
```

```python
# inputs
sol = Solution()
nums = [1, 1, 2, 2]
print(sol.permuteNotUnique(nums)) # today's problem solution
print(sol.permuteUnique(nums))
```

```python
# outputs
[[1, 1, 2, 2], [1, 1, 2, 2], [1, 2, 1, 2], [1, 2, 2, 1], [1, 2, 1, 2], [1, 2, 2, 1], [1, 1, 2, 2], [1, 1, 2, 2], [1, 2, 1, 2], [1, 2, 2, 1], [1, 2, 1, 2], [1, 2, 2, 1], [2, 1, 1, 2], [2, 1, 2, 1], [2, 1, 1, 2], [2, 1, 2, 1], [2, 2, 1, 1], [2, 2, 1, 1], [2, 1, 1, 2], [2, 1, 2, 1], [2, 1, 1, 2], [2, 1, 2, 1], [2, 2, 1, 1], [2, 2, 1, 1]]
[[1, 1, 2, 2], [1, 2, 1, 2], [1, 2, 2, 1], [2, 1, 1, 2], [2, 1, 2, 1], [2, 2, 1, 1]]
[[1, 1, 2, 2], [1, 2, 1, 2], [1, 2, 2, 1], [2, 1, 1, 2], [2, 1, 2, 1], [2, 2, 1, 1]]
```

还是挺好玩的。

## Conclusion

- 感觉今天大多数做题的时候都是兴趣使然，不过能看到装饰器的使用变得熟练，个人还是比较开心的。