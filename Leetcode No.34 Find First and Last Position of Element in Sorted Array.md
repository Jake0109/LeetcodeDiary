# Leetcode No.34 Find First and Last Position of Element in Sorted Array

## Description

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

## My Solution

首先是逃课版：

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        try:
            end = start = nums.index(target)
        except ValueError as e:
            return [-1,-1]
        
        while end + 1 < len(nums) and nums[end+1] == target:
            end += 1
        
        return [start,end]
```

然后好好写：

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums:
            return [-1,-1]
        start = 0
        end = len(nums)-1
        target_index = None
        while start <= end:
            mid = (start+end)//2
            if nums[mid] == target:
                target_index = mid
                break
            elif nums[mid] < target:
                start = mid + 1
            else:
                end = mid - 1
        else:
            return [-1,-1]

        first = last = target_index
        while first - 1 >= 0 and nums[first - 1] == target:
            first -= 1
        while last + 1 < len(nums) and nums[last + 1] == target:
            last += 1
        return [first,last]
```

就是一个二分查询，也算符合要求的O(logn)。

## Reflection & Polishing-up

额...因为最终，我的这个二分法的Approach也不是非常的快，所以原来准备去讨论区里面找找有没有什么大神的代码来着...结果，我放一些代码看一下吧~~不能我一个人瞎~~

```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if target not in nums:
            return[-1,-1]
        list = []
        ans = 0
        for i in nums:
            if(i==target):
                list.append(ans)
                ans+=1
            else:
                ans+=1
                
        return[min(list),max(list)]
```

时间复杂度O(n)，而且ans这个变量我没看到任何的用，我不知道是因为是python2还是什么原因，这个循环写的太烂了...要么就`for i in range(nums): nums[i]...`要么就`for i,num in enumerate(nums):...`简洁易懂的方法那么多，为什么就选了这么一个难懂，复杂的方法呢...

## Conclusion

- 代码能力不用说，规范也很重要，就算python是个很自由的语言，在工程 里面，一个杂乱无章的代码块，肯定是让人想要砸键盘的。
- 正如我之前那样，所有遍历对象都是`i`，这显然是对读代码的人不友好的，出题方好心好意把nums的list给出来了，不好好用`for num in nums:`意欲何为呢？
- 今天看了一点东西有点火大，请各位看官见谅。

