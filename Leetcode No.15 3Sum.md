# Leetcode No.15 3Sum

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## My solution

一个有点笨的三重循环。

```python
def threeSum(nums):
    length = len(nums)
    list = []
    for i in range(length):
        for j in range(i+1,length):
            for k in range(j+1,length):
                if nums[i]+nums[j]+nums[k] == 0:
                    res = [nums[i],nums[j],nums[k]]
                    res.sort()
                    if res not in list:
                        list.append(res)
    return list
```

试着改了一下：

```python
def threeSum(nums):
    length = len(nums)
    list = []
    for i in range(1,length):
        for j in range(i+1,length):
            tmp = nums[i]+nums[j]
            if -tmp in nums[:i] and sorted([-tmp,nums[i],nums[j]]) not in list:
                list.append(sorted([-tmp,nums[i],nums[j]]))
    return list
```

但是in操作的时间复杂度是O(n)所以...本质上没区别...于是乎，博主的solution就一直 Time Limit Exceeded。

## Reflection & Polishing-up

无可奈何，博主就去看看大佬们怎么写的吧.

```python
class Solution(object):
    def threeSum(self, nums):
        res = set()
        nums.sort()
        length = len(nums)
        for i in range(length-2):
            if nums[i]>0: break
            if i>0 and nums[i]==nums[i-1]: continue
            l, r = i+1, length-1
            while l<r:
                total = nums[i]+nums[l]+nums[r]
                if total<0:
                    l+=1
                elif total>0:
                    r-=1
                else:
                    res.add(tuple([nums[i], nums[l], nums[r]]))
                    l+=1
                    r-=1
        return list(res)
```

我觉得，有时候确实，能把这类题目先排序以后，可能会有更加不同的但是更优的思路，但是排序本身是要花费时间的，不过一个内置的sort函数的时间复杂度仅为O(nlogn)何尝不可呢，~~比起我这个O(n^3)的算法~~不过估计就算我先使用了排序，可能也不能写这么好吧。

另外一点是，我也想到了set元素互异的特性，也准备使用来着，可是没能实现。

> 大家需要知道的一点是set（集合）是通过hash表实现的，hash算法就是对元素进行一系列操作得到一个字符串，而当元素的内容变化的时候，hash算法得到的值也会有变化，而hash表就是基于这个算法实现的一种数据结构。而如果元素的内容变化的话，hash表索引就会出现混乱，所以set中的元素必须是immutable（不可变）的，那么博主的尝试自然就失败了。

不过这里用tuple的思路确实非常的好，并且从两边逼近的这种思路，确实博主还没有很深刻的体会，所以不会自然而然地想到去用，这点学习到了。

另外一个算法：

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        
        counts, res = {}, set()
        for n in nums: counts[n] = counts.get(n, 0) + 1
        if counts.get(0,0) >= 3: res.add((0,0,0)) # edge case
        
        # remove repeats that occur over 3 times
        for k,v in counts.items(): 
            if v > 2: counts[k] = 2 
        nums = [n for key,v in counts.items() for n in v*[key]]
        
        for i, a in enumerate(nums):
            for b in nums[i+1:]:
                c = -a -b # constraint: a + b + c = 0
                if c in counts and c!= a and c!= b: 
                    res.add(tuple(sorted([a,b,c])))
        
        return [list(n) for n in res]
```

其实这才是讨论区第一，一开始看，着实有点没看懂，现在差不多理解了，注释其实也已经给的差不多了。首先是将出现的edge case：[0,0,0]找出来，接着将出现三次的元素减少至两个，因为出现三个并不影响最终结果，除了已经排除的[0,0,0]，最后根据上面得到的新nums，以及counts的内容得出最终的结果。

首先必须要说的是，在set和list中，`in`这个操作的时间复杂度是不一样的，list中是O(n)，而set中则是O(1)，所以这个确实是一个时间复杂度为O(n^2)的好算法。

另外`nums = [n for key,v in counts.items() for n in v*[key]]`，像是这种类似匿名算法的表达方式，我也还是不熟练，或者说，没有被说的话，自己是不会想到要用的，确实用这个的话，代码会非常简洁。