# Leetcode No.75 Sort Colors

## Description

Given an array with *n* objects colored red, white or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note:** You are not suppose to use the library's sort function for this problem.

**Example:**

```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Follow up:**

- A rather straight forward solution is a two-pass algorithm using counting sort.
  First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
- Could you come up with a one-pass algorithm using only constant space?

## My Solution

这不就是排序吗？整那么高大上，一行搞定：

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums.sort()
```

~~别打了别打了，我知道错了~~

这肯定就是计数排序嘛，这已经不是暗示了，明示！总共3个数，排序！连初始化计数数列的步骤都省去了。

自己先按照直觉写了一点东西：

```python
class Solution:
    def sortColors(self, nums):
        count = {0:0, 1:0, 2:0}

        for num in nums:
            count[num] += 1

        res = []

        for num in count.keys():
            res += [num] * count[num]

        return res
```

那么问题在哪里呢，~~没错，自己回头随便看一眼就知道问题很多~~

先把正确的给出来：

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        count = {0:0, 1:0, 2:0}

        for num in nums:
            count[num] += 1

        nums.clear()

        for i in range(3):
            while count[i]:
                nums.append(i)
                count[i] -= 1
```

## Reflection & Polishing-up

OK，开始我处刑我自己：

```python
class Solution:
    def sortColors(self, nums):
        count = {0:0, 1:0, 2:0}

        for num in nums:
            count[num] += 1

        res = []
		
        # 2.字典是无顺序的，也就是说，用这种方式遍历，有可能出现[0,1,2]以外的顺序。
        for num in count.keys():
            res += [num] * count[num]

        # 1.在原数列nums上更改数据，并不需要返回值
        return res
    # 3.大方向上，不推荐使用字典来保存计数，因为范围不好确定
```

还有一个更适合本题的做法：Two-Pointer

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if not nums: return []
        # pointers to keep track of where the current sorting is
        i, left, right = 0, 0, len(nums)-1
        while i <= right:
            if nums[i] == 0:
                # swap left
                nums[left], nums[i] = nums[i], nums[left]
                left += 1; i += 1
            elif nums[i] == 2:
                # swap right
                nums[right], nums[i] = nums[i], nums[right]
                right -= 1
            else: i += 1
```

可以用这么一个表来表示：

| 普适       | -->      | 特殊            |
| ---------- | -------- | --------------- |
| 普通的排序 | 计数排序 | Two-Pointer排序 |

## Conclusion

- 嗯，普适和定制，感觉是个永远的话题，感觉能作为一个哲学话题研究下去。
- 这个题目中，肯定是Two-Pointer这个方案更优，但是如果颜色的数量超过3，那么这个方法就不再有效，那么最优解就是计数排序（更普通的，用列表的版本）。但是抽象到最后，都是排序。
- 那么在我的这种场景中，结论只能是：在能够想到普适的Solution时，积极求变，并寻求更好的解。
  - 能够想到普适的Solution，就代表可以联系已知和未知。
  - 寻求更优的解，是为了学习。