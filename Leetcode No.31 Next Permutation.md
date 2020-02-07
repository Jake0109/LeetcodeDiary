# Leetcode No.31 Next Permutation

## Description

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1
```

## My Solution

这个思路其实挺难找的，虽然直接给我人做，可能能很快写出来，但是要写成代码来实现，着实有着不小的难度。但是最终还是顺着我自己的脑回路（思考方式）写了出来：

```python
class Solution:
    def nextPermutation(self,nums) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if len(nums) == 2 or nums == sorted(nums,reverse=True):
            nums.reverse()
            return None
        sort_nums = list(set(sorted(nums)))
        sort_nums.append(sort_nums[0])
        for i in range(len(nums)-2,-1,-1):
            if nums[i:] == sorted(nums[i:],reverse=True):
                continue
            else:
                tmp = sort_nums.index(nums[i])+1
                while sort_nums[tmp] not in nums[i+1:]:
                    tmp += 1
                index = nums.index(sort_nums[tmp],i+1)
                nums.insert(i,nums.pop(index))
                nums[i+1:] = sorted(nums[i+1:])
                break
```

这是一道medium题，我现在大概了解Leetcode题目难度跟我的水平的对应关系了，如果是见过的题目，medium就是看一眼，半个小时内做完的题目，如果是没见过的，大概就要花将近一个小时。~~你放屁，你这题花了快一个半小时了~~如果hard是见过的，大概也是半个小时到一个小时解出来，没见过则有概率解不出来。

## Reflection & Polishing-up

看了一下official Solution，我的是"Approach 2: Single Pass Approach"这种方法，而discussion里，基本也都是类似的方法或者思路。

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if len(nums) < 2:
            return
        
        for slow in range(len(nums) - 2, -1, -1):
            fast = len(nums) - 1
            smallest = fast
            while fast > slow:
                if nums[fast] > nums[slow]:
                    if nums[fast] < nums[smallest] or nums[smallest] <= nums[slow]:
                        smallest = fast
                fast -= 1
            if nums[smallest] > nums[slow]:
                nums[smallest], nums[slow] = nums[slow], nums[smallest]
                nums[slow+1:] = sorted(nums[slow+1:])
                return
            
        nums.sort()
```

这个用的快慢指针实现的Single Pass Approach。我好像还不太会用快慢指针，这个似乎不是没有意识，而是因为没有用过，所以没有意识。

还有一个比较有意思的solution，虽然bisect这个库我没用过：

```python
import bisect
class Solution:
    
    def reverse(self,nums,low,high):
        while low<=high:
            nums[low],nums[high] = nums[high],nums[low]
            low+=1
            high-=1
    
    def nextPermutation(self, nums: List[int]) -> None:
        length = len(nums)
        i = length-2
        index = None
        while i>=0:
            if nums[i]<nums[i+1]:
                index = i
                break
            i = i-1
        if index==None:
            self.reverse(nums,0,length-1)
        else:
            self.reverse(nums,index+1,length-1)
            swap_index = bisect.bisect_right(nums,nums[index],index+1,length-1)
            nums[index],nums[swap_index] = nums[swap_index],nums[index]
```

提供了一个更好的找到关键节点index的方法，那么看了这个我觉得我的这个代码也可以优化一下：

```python
def nextPermutation(nums) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    if len(nums) == 2 or nums == sorted(nums, reverse=True):
        nums.reverse()
        return None
    sort_nums = list(set(sorted(nums)))
    sort_nums.append(sort_nums[0])
    i = len(nums)-2
    while i>0:
        if nums[i]<nums[i+1]:
            break
        i -= 1
    tmp = sort_nums.index(nums[i]) + 1
    while sort_nums[tmp] not in nums[i + 1:]:
        tmp += 1
    index = nums.index(sort_nums[tmp], i + 1)
    nums.insert(i, nums.pop(index))
    nums[i + 1:] = sorted(nums[i + 1:])
```

然后就性能变差了...runtime从40ms增加到了44ms...我现在能想到的就是'=='和'>'之间的差距了...但是我感觉后面一个代码的可读性更好一点...

## Conclusion

一下子还想不太出来总结点什么，虽然这道题我花了不少时间：

- 对于技术，或者说一些思想就是要多用，多实践。
- 就像世界观之于方法论，要执行了方法论，才会加深世界冠的理解。当然如何践行方法论是我到现在都没有参透的课题。