# Leetcode No.26 Remove Duplicates from Sorted Array

## Description

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

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
    def removeDuplicates(self,nums):
        dict = {}
        for val in nums:
            if val in dict:
                dict[val] += 1
            else:
                dict[val] = 1
        return len(dict)
```

这就是不看clarification的结果...

```python
class Solution:
    def removeDuplicates(self,nums):
        dict = {}
        for i,x in enumerate(nums):
            if x in dict:
                dict[x] += [i]
            else:
                dict[x] = [i]
        for i in dict.keys():
            if len(dict[i])>=2:
                for j in range(len(dict[i])-1):
                    nums.remove(i)
        return len(nums)
```

莫名其妙加了很多戏...返回一个int的话，本身不是什么难事，但是...说好的不要在函数里面改全局变量呢...说好的编程的优雅呢。

## Reflection & Polishing-up

看了一下Official Solution，我只能说...投机取巧...行吧，我自己也来实现一遍。

```python
class Solution:
    def removeDuplicates(self,nums):
        if not nums: return 0
        res = 1
        tmp = nums[0]
        for i in range(1,len(nums)):
            if nums[i] != tmp:
                nums[res] = nums[i]
                res += 1
                tmp = nums[i]
        return res
```

就跟写出来的一样，没什么好说的...很不喜欢的实现方式。

```python
def removeDuplicates(nums):
    y = nums[0]
    x = 1
    length = len(nums)

    while x < length:
        if (y == nums[x]):
            nums.pop(x)
            length -= 1
        else:
            y = nums[x]
            x += 1
    return length
```

看了一下，这个实现的还挺好的，简洁明了，在考虑到改变参数以后的变动以后，设计出的solution，很nice。