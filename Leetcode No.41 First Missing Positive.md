# Leetcode No.41 First Missing Positive

## Description

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Note:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

## My Solution

```python
class Solution:
    def firstMissingPositive(self,nums):
        if not nums:
            return 1

        maxNum = nums[0]
        hashmap = {}  # use hashmap to accelerate

        for num in nums:
            hashmap[num] = 1
            if num > maxNum:
                maxNum = num

        for num in range(1,maxNum):
            if not hashmap.get(num):
                return num
        else:
            return max(maxNum + 1, 1)
```

说实话，这一道hard题确实没什么难度，准确来说我写的是个O(2n)的函数。不过我也不是一次过，中间也有一些场景没有考虑到，比如，当nums中只有负数，那么我只会返回max(nums)+1而不是1。

## Reflection & Polishing-up

关于上面提到的场景，我想到Python中装饰器的用法，这里来玩一玩骚操作。

```python
def isPositive(func):
    def wrapper(nums):
        res = func(nums)
        if res < 0:
            return 1
        return res
    return wrapper

@isPositive
def firstMissingPositive(nums):
    if not nums:
        return 1

    maxNum = nums[0]
    hashmap = {}  # use hashmap to accelerate

    for num in nums:
        hashmap[num] = 1
        if num > maxNum:
            maxNum = num

    for num in range(1, maxNum):
        if not hashmap.get(num):
            return num
    else:
        return maxNum + 1
```

第一次实战用decorator，过程之艰辛就不在这里吐苦水了，多用用就熟悉了。

## Conclusion

- 实践不一定是检验真理的唯一标准，但是不实践，一定检验不了。说到底，多做，多找机会做，就能成长。
- 在我看来，装饰器是python中最具特色的特性~~那是因为你python不精~~，想要用好python，装饰器，生成器，迭代器都还需要更加精进。