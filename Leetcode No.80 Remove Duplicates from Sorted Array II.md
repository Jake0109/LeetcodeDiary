# Leetcode No.80 Remove Duplicates from Sorted Array II

## Description

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most *twice* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,1,2,3,3],

Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## My Solution

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        has_second = False
        pre = None

        i = 0
        while i < len(nums):
            if pre is None:
                pre = nums[i]
                i += 1
                continue
            if pre == nums[i]:
                if not has_second:
                    has_second = True
                    i += 1
                    continue
                else:
                    nums.pop(i)
            else:
                pre = nums[i]
                has_second = False
                i += 1

        return len(nums)
```

之前已经写过类似的了，没什么多说的，挺简单的。

## Reflection

刚写到这，一个面试就过来了，东西还没看完...但是估计这会是看不下去了。先贴给你们看看吧：

```python
class Solution:
    def removeDuplicates(self, n: List[int]) -> int:
    	L, j = len(n), 0
    	if L <= 1: return L
    	for i in range(L-2):
    		if n[i] < n[i+2]: n[j], j = n[i], j + 1
    	n[j], n[j+1], j = n[-2], n[-1], j + 2
    	return j



class Solution:
    def removeDuplicates(self, n: List[int]) -> int:
    	L = len(n)
    	if L <= 2: return L
    	j, a = 1 + (n[0] == n[1]), n[-1] != n[-2]
    	for i in range(1,L-1):
    		if n[i] != n[i-1]:
    			n[j], j = n[i], j + 1
    			if n[i] == n[i+1]: n[j], j = n[i], j + 1
    	if a: n[j], j = n[-1], j + 1
    	return j
		
		
		
class Solution:
    def removeDuplicates(self, n: List[int]) -> int:
    	L, j = len(n), 2
    	for i in range(2,L):
    		if n[i] > n[j-2]: n[j], j = n[i], j + 1
    	return j
```

## Conclusion

- 别用惯性思维思考，面试的时候问我list的去重，我直接用了这里的思路，但是没说要返回原list，当时被指出来还有点小慌。