# Leetcode No.88 Merge Sorted Array

## Description

Given two sorted integer arrays *nums1* and *nums2*, merge *nums2* into *nums1* as one sorted array.

**Note:**

- The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.
- You may assume that *nums1* has enough space (size that is greater or equal to *m* + *n*) to hold additional elements from *nums2*.

**Example:**

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

## My Solution

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        while len(nums1) > m:
            nums1.pop()
        nums1.extend(nums2)
        nums1.sort()
```

~~偷个懒，给爷爬~~

还是老老实实用双指针做吧。

```python
class Solution:
    def merge(self, nums1, m: int, nums2, n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        while len(nums1) > m:
            nums1.pop()

        i = 0
        j = 0
        while i < m + n and j < n:
            if i == len(nums1):
                break

            if nums2[j] < nums1[i]:
                nums1.insert(i,nums2[j])
                j += 1
            i += 1

        if j < n:
            nums1.extend(nums2[j:])
```

## Reflection & Polishing-up

我想不出来了...这个太简单了...看来看去，就那么几个...

## Conclusion

- 原谅我，真的想不出来了。