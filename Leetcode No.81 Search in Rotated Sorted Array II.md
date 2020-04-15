# Leetcode No.81 Search in Rotated Sorted Array II

## Description

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

**Follow up:**

- This is a follow up problem to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), where `nums` may contain duplicates.
- Would this affect the run-time complexity? How and why?

## My Solution

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        return True if target in nums else False
```

没搞二分法，因为一点都不简洁，虽然在大规模数据中的效率更高。

## Reflection & Polishing-up

可以更简单

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        return target in nums
```

或者

```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        return target in set(nums)
```

都是O(n)的时间复杂度。

看到了二分法的了：

```python
class Solution:
    def search(self, n: List[int], t: int) -> bool:
    	L = len(n)
    	if L <= 1: return t in n
    	if t in [n[0],n[-1]]: return True
    	for i in range(L-1):
    		if n[i] == t: return True
    		if n[i] > n[i+1]: break
    	if n[i] <= n[i+1]: return False
    	I = (bisect.bisect_left(n[i+1:L] + n[:i+1], t) + i + 1) % L
    	return n[I] == t
```

旨在找到rotated的点，然后再二分，说实话：挺不能理解的，还是个O(n)的意思呗。

## Conclusion

- 我觉得强行使用二分法是一个降低效率的操作。
- 使用二分法会让本来简洁的代码变得冗杂，我也不喜欢这样。