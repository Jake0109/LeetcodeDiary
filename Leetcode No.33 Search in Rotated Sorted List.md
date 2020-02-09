# Leetcode No.33 Search in Rotated Sorted List

## Description

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

## My Solution

看到这题就直接想到了index()函数，为什么：

- index的时间复杂度是O(1)
- 内置函数，不需要我们添加多余的功能

但是index()在寻找不到target的时候，会报ValueError，连程序都执行不下去了，还怎么返回-1呢，那就要用到异常处理，那么异常处理在这道问题里面的解决方法是这样的：

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        try:
            res = nums.index(target)
        except ValueError as e:
            return -1
        return res
```

那么这种逃课式的方法，我把这题搞定了。

## Reflection & Polishing-up

其实一开始我想到的是二分查找，但是二分查找可以在这种Rotated List 中使用与否，我依然存疑，于是，我决定再试一试：

```python
class Solution:
    def search(self,nums, target: int) -> int:
        if not nums:
            return -1
        start = 0
        end = len(nums)-1
        while start <= end:
            mid = (start+end)//2
            if start == end and nums[mid] != target:
                return -1
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                if nums[mid] < target < nums[end]:
                    end = mid - 1
                else:
                    start = mid + 1
            else:
                if nums[start] < target < nums[mid]:
                    end = mid - 1
                else:
                    start = mid + 1
```

嗯...这样还是AC不了，因为除法以后取整的问题，总是感觉脑袋里面缺一点。不纠结了，去discussion里面看看吧，然后就看到清一色的二分...：

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        first=0
        last=len(nums)-1
        
        #base condition met: return index value
        while first<=last:
            mid=(first+last)//2
            
            if nums[mid]==target:
                return mid
            
            #right half is in sorted order
            if nums[mid]<nums[last]:
                
                #search in second half
                if nums[mid]<target<=nums[last]:
                    first=mid+1
                #search in first half  
                else:
                    last=mid-1
            #left half is in sorted order        
            else:
                #search in first half
                if nums[first]<=target<nums[mid]:
                    last=mid-1
                #search in second half   
                else:
                    first=mid+1
                    
        #not found   
        return -1
```

额，没想到还需要确认左右边是否sorted...理论上来说，这个实现方法应该是比我的那个快的，因为...我那个不论什么样的list都肯定顶能够返回正确的结果，而这种Approach是特化的。果真如此...

## Conclusion

总结一下，就是一些基本功还不到位吧。