# Leetcode No.35 Search Insert Index

## Description

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```

## My Solution

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        if target < nums[0]:
            return 0
        elif target > nums[-1]:
            return len(nums)
        
        index = 0
        while nums[index] < target:
            index += 1
        return index
```

嗯...是个easy的题目，确实也不用花太多时间...一个linear的方法，我来试试看二分法

```python
class Solution:
    def searchInsert(self, nums, target: int) -> int:
        if target < nums[0]:
            return 0
        elif target > nums[-1]:
            return len(nums)
        if len(nums) == 1:
            return 0

        start, end = 0, len(nums) - 1
        while end > start:
            mid = (start + end) // 2
            if mid == start:
                if target > nums[mid]:
                    return mid + 1
                else:
                    return mid
            if target == nums[mid]:
                return mid
            elif target > nums[mid]:
                start = mid
            else:
                end = mid
```

嗯，一下子效率就上去了。

## Reflection & Polishing-up

相信很多人看到这一题，第一反应都是index()但是，当目标不存在时，有需要做一个逃不了课的过程，所以，请锻炼自己的coding。这个还真没看到什么更好的Approach，因为确实是一个简单的问题...

## Conclusion

- 学会用while，而不只是for，这个是我自己的毛病。~~如果不介意博主说点过去的事情，~~博主的coding time主要集中在大一的时候，那个时候的问题，大多数用for都能解决，或者说只想到for来解决，几乎没有用while的意识，现在越来越熟练的区分使用while和for的场景是我个人的一个比较大的提升。
- 先实现再优化，自己敲代码，还是在工程中，都是这样，不需要什么完美主义，那只是精神洁癖者的借口。It is always a truth that Failures contribute to Success! 