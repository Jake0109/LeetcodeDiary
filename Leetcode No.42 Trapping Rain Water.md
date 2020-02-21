# Leetcode No.42 Trapping Rain Water

## Description

Given *n* non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

![img](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)
The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. **Thanks Marcos** for contributing this image!

**Example:**

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

## My Solution

```python
from collections import defaultdict


class Solution:
    def trap(self, heights):
        if not heights or len(heights) <= 2:
            return 0

        climaxes = defaultdict(list)  # to log the index and height of climaxes
        length = len(heights)

        for i, height in enumerate(heights):
            if i == 0 and height >= heights[1]:
                climaxes[height].append(i)
                continue
            if i == length - 1 and height >= heights[i - 1]:
                climaxes[height].append(i)
                break
            if height >= heights[i - 1] and height >= heights[i + 1]:
                climaxes[height].append(i)

        sorted_climaxes = sorted(climaxes.items(), reverse=True)
        left_border = sorted_climaxes[0][1][0]
        right_border = sorted_climaxes[0][1][-1]
        needed_climax = defaultdict(list)
        
		# to build the dict: key = height value:list that contains the range it need to 		# travelsal
        for i,climax in enumerate(sorted_climaxes):
            
            # if the highest climax is single, it will be hard to handle
            if i == 0:
                needed_climax[climax[0]].append([left_border,right_border])
                continue
            
            temp_left = None
            temp_right = None
            if climax[1][0] < left_border:
                temp_left = climax[1][0]
                needed_climax[climax[0]].append([temp_left,left_border])
                left_border = temp_left
            if climax[1][-1] > right_border:
                temp_right = climax[1][-1]
                needed_climax[climax[0]].append([right_border,temp_right])
                right_border = temp_right
        
        result = 0
        for height,indexes in needed_climax.items():
            for index in indexes:
                for i in range(index[0]+1,index[1]):
                    result = result + max(0, height - heights[i])

        return result
```

这个感觉有点高中的多次求导的那种意思...总之先AC了，过程中也要求自己符合规范，进行名命，排版。

## Reflection & Polishing-up

在官方的solutions里面看到了可以用DP，也可以用stack，那我明天用这两种方法实现了看一看。

### DP：

```python
class Solution:
    def trap(self, heights):
        if not heights:
            return 0
        
        result = 0
        left_max = []
        right_max = [0 for i in range(len(heights))]
        length = len(heights)

        left_max.append(heights[0])
        for i in range(1, length):
            left_max.append(max(heights[i], left_max[i - 1]))

        right_max[length - 1] = heights[length - 1]
        for i in range(length - 2, -1, -1):
            right_max[i] = max(heights[i], right_max[i + 1])

        for i, height in enumerate(heights):
            result += min(left_max[i],right_max[i]) - height

        return result
```

其中的主要思想是这样的：

![Dynamic programming](https://leetcode.com/problems/trapping-rain-water/Figures/42/trapping_rain_water.png)

**Algorithm**

- Find maximum height of bar from the left end upto an index i in the array \text{left\_max}left_max.
- Find maximum height of bar from the right end upto an index i in the array \text{right\_max}right_max.
- Iterate over the **height** array and update ans:
  - Add min(max_left[i], max_right[i]) - height[i] to *ans*.~~原来原文里面用的是max_left/right啊...~~

### Stack

```python
    def trap_stack(self, heights):
        if not heights or len(heights) < 2:
            return 0

        stack = []
        result = 0
        for i, height in enumerate(heights):
            while stack and height > heights[stack[-1]]:
                bottom_height = heights[stack.pop()]
                if not stack:
                    break
                    
                height_diff = min(height, heights[stack[-1]]) - bottom_height
                width = i - stack[-1] -1
                result += height_diff * width
                
            stack.append(i)

        return result
```

这个感觉是做到目前为止最吃力的一个，可能我stack本来就用的不是很好，一旦遇到比较复杂的情况，可能脑子就会转不过来吧><。后面的 Two Pointer 是最快的，但是今天已经花了将近两个小时的时间了，明天再继续，这题是第一个让我花了3天研究的题目啊...

### Two Pointer

```python

```

## Conclusion

