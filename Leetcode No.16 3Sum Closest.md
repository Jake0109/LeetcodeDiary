## Leetcode No.16 3Sum Closest

## Description

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

## My Solution

和昨天的题目有点类似，所以可以试试看能不能用昨天那种优秀的思路。

首先想一下最后一种运用到diction的思路：

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

我想了一下，因为这个解法的特殊性（用到了diction），我觉得在本题中应用不了，所以在本题中运用不能。

所以我就使用了昨天polishing-up中第一种，两边逼近的方法来完成。

```python
class Solution:
    def threeSumClosest(self,nums:list,target):
        nums.sort()
        CLosestSum = nums[0] + nums[1] + nums[2]
        minDiff = abs(CLosestSum - target)
        for i in range(len(nums)-2):
            l,r = i+1,len(nums)-1
            while l<r:
                total = nums[i] + nums[l] + nums[r]
                tmp = abs(total-target)
                if tmp < minDiff:
                    minDiff = tmp
                    CLosestSum = total
                if total < target:
                    l += 1
                elif total > target:
                    r -= 1
                else:
                    return target
        return CLosestSum   
```

就这样完成了一个时间复杂度O(n^2)的solution，对于能够运用之前学过的知识还是非常开心的。

## Reflection & Polishing-up

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        l = len(nums)
        if l < 3:
            return None
        nums.sort()
        result = nums[0] + nums[1] + nums[2] - target
        index = (0, 1, 2)
        lastk = -1
        for j in range(l - 1, 1, -1):
            temp = nums[j] + nums[j - 1] + nums[j - 2] - target
            if temp < 0:
                if abs(temp) < abs(result):
                    result = temp
                    index = (j - 2, j - 1, j)
                break

            for i in range(j - 1):
                left = i
                right = j
                temp = nums[i] + nums[i + 1] + nums[j] - target
                if temp > 0:
                    if abs(temp) < abs(result):
                        result = temp
                        index = (i, i + 1, j)
                    break

                if left < lastk < right:
                    k = lastk
                else:
                    k = (left + right) // 2
                while left < k < right:
                    temp = nums[i] + nums[j] + nums[k] - target
                    if abs(temp) < abs(result):
                        result = temp
                        index = (i, k, j)
                    if temp == 0:
                        return target
                    if temp > 0:
                        right = k
                    elif temp < 0:
                        left = k
                    k = (left + right) // 2
                lastk = k
        sum = nums[index[0]] + nums[index[1]] + nums[index[2]]
        return sum
```

是个很好的思路，灵活地将二分查找运用到了这个题目中，如果我分析的没有问题的话，这个算法的时间复杂度应该是O(nlogn)所以自然会比我的算法快很多。