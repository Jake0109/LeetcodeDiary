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

pass

## Conclusion

