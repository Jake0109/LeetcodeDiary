# Leetcode No.84 Largest Rectangle in Histogram

## Description

Given *n* non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

 

![img](https://assets.leetcode.com/uploads/2018/10/12/histogram.png)
Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

 

![img](https://assets.leetcode.com/uploads/2018/10/12/histogram_area.png)
The largest rectangle is shown in the shaded area, which has area = `10` unit.

 

**Example:**

```
Input: [2,1,5,6,2,3]
Output: 10
```

## My Solution

考察完自己想的两个特例以后的失败作：

```python
class Solution:
    def largestRectangleArea(self, heights) -> int:

        if not heights:
            return 0

        left = 0
        right = len(heights) - 1

        res = 0

        while left <= right:
            Min = min(heights[left:right+1])
            area = (right - left + 1) * Min
            min_index = heights.index(Min)
            if area > res:
                res = area

            if heights[left] < heights[right]:
                left += 1
            else:
                right -= 1

        return res
```

因为直觉的话就是遍历所有的[i,j]组合，然后取其中的最小值算出四边形面积，那就是O(n^3)太慢了，所以想要做一个O(n^2)的算法，第一反应是双指针，但是失败了。

然后想到了这个方法，也是O(n^2)：

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        res = 0

        for i,height in enumerate(heights):
            left = i
            right = i

            while left >= 0:
                if heights[left] < height:
                    break
                left -= 1
            while right < len(heights):
                if heights[right] < height:
                    break
                right += 1

            area = (right-left-1)*height
            res = max(res,area)

        return res
```

但是也超时了...

所以我决定去看看官方的题解，发现了不少别的解法，我明天全部实现一次然后再更新吧。