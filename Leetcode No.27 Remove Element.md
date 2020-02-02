# Leetcode No.27 Remove Element

## Description

Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## My Solution

啊...又来了，要直接操作参数的函数...

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        while val in nums:
            nums.remove(val)
        return len(nums)
```

其实就可以了...去看了看Official Solution，一个使用Java实现的，第二个，也没有对列表操作，为啥python就要改列表呢....

## Reflection & Polishing-up

去讨论区看看：

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i=0
        while i < len(nums) : 
            if(nums[i]==val) : 
                del nums[i]
            else : 
                i+=1
        return len(nums)
```

收获：

​		在要涉及减少迭代器的数量的操作的循环，用while最佳（目前我的理解），循环的步数自己来按照条件写。