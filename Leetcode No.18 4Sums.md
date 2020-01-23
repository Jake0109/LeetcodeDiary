# Leetcode Diary No.18 4Sums

## Description

Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums` such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

## My Solution

这里可以不用3Sum的思路，转用2Sum的思路会更高效，使用3Sum的思路的话时间复杂度是O(n^3)，而使用2Sum的思路的话只需O(n^2)如何一步步的实现，我们现在开始。

```python
def fourSum(nums,target):
    length = len(nums)
    dict = {}
    res = []
    for i in range(nums-1):
        for j in range(i+1,nums):
            tmp = nums[i] + nums[j]
            pass
            dict[tmp] = (i,j)
    return res
```

这里首先勾勒出这个算法的大致框架，是一个二重循环，中间建立一个dictionary，key为两数之和，value为这两个数的index。

```python
def fourSum(nums,target):
    length = len(nums)
    dict = {}
    res = []
    for i in range(length-1):
        for j in range(i+1,length):
            sum2 = nums[i] + nums[j]
            pass
            if sum2 not in dict:
                dict[sum2] = [[i,j]]
            else:
                dict[sum2].append([i,j])
    print(dict)
    return res
```

当key重复的时候的更新。

```python
def fourSum(nums, target):
    length = len(nums)
    dict = {}
    res = set()
    for i in range(length - 1):
        for j in range(i + 1, length):
            sum2 = nums[i] + nums[j]
            tmp = dict.get(target - sum2)
            if tmp:
                for k in tmp:
                    if i in k or j in k:
                        continue
                    res.add((tuple(sorted(k + [i, j]))))
            if sum2 not in dict:
                dict[sum2] = [[i, j]]
            else:
                dict[sum2].append([i, j])
    reslut = []
    return reslut
```

开始为了返回值添加set中的数据。

```python
class Solution:
    def fourSum(self,nums, target):
        length = len(nums)
        dict = {}
        res = set()
        for i in range(length - 1):
            for j in range(i + 1, length):
                sum2 = nums[i] + nums[j]
                tmp = dict.get(target - sum2)
                if tmp:
                    for k in tmp:
                        if i in k or j in k:
                            continue
                        res.add((tuple(sorted(k + [i, j]))))
                if sum2 not in dict:
                    dict[sum2] = [[i, j]]
                else:
                    dict[sum2].append([i, j])
        result = []
        for i in res:
            addition = sorted([nums[j] for j in i])
            if addition not in result:
                result.append(sorted([nums[j] for j in i]))
        return result
```

完成。

## Reflection & Polishing-up

这个Problem的完成可以说是之前一段时间练习的结果了。

- 在2Sum的问题中：运用dictionary（hash table）来减少查询的时间复杂度。
- 在3Sum的问题中：
  - 用set来解决互异问题。
  - 用tuple来解决list对象不能被hash的问题。
  - 学着使用list comprehension来增加代码可读性。

这次没有在submission中看到什么优秀的代码，大多python代码都是沿用3Sum的思路做出来的O(n^3)的代码，别的语言的O(n^2)也都跟我的实现方法大同小异，这里就不列出了。~~第一次这么有成就感~~