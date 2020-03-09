# Leetcode No.57 Insert Interval

## Description

Given a set of *non-overlapping* intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## My Solution

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        
        insert_index = None
        for i,interval in enumerate(intervals):
            if interval[0] > newInterval[0]:
                insert_index = i
                break
        if insert_index is None:
            intervals.append(newInterval)
        else:
            intervals.insert(insert_index, newInterval)

        print(intervals)
        merged = []
        for interval in intervals:
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)

            else:
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

我肯定要再试试的。这个方案肯定是非常的没有效率的，因为需要先插入，再做一遍上一道题目的操作。

## Reflection & Polishing-up

```python
class Solution:
    def insert(self, intervals, newInterval):
    	i = bisect.bisect_left(intervals, newInterval)  # 二分查找合适的插入点
        while i < len(intervals) and intervals[i][0] <= newInterval[1]:
            newInterval[1] = max(newInterval[1], intervals.pop(i)[1])
        intervals.insert(i, newInterval)  # 合并后插入
        if i - 1 >= 0 and intervals[i - 1][1] >= intervals[i][0]:  # 如果左边也能合并则合并
            intervals[i - 1][1] = max(intervals[i - 1][1], intervals.pop(i)[1])
        return intervals
```

怎么可能拖到明天呢，今天就要看！这个是去中文社区看到的，确实很优秀，虽然运行效率甚至不如我的那个高。但是提醒了我：

- 这中间需要查找位置
- 查找不一定要遍历，也可以二分查找。

另外看到一些别人的代码，也能发现一些自己代码中可以修改的地方

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        
        insert_index = None
        for i,interval in enumerate(intervals):
            if interval[0] > newInterval[0]:
                insert_index = i
                break
        else:
            insert_index = len(intervals)
		intervals.insert(insert_index, newInterval)
            
        merged = []
        for interval in intervals:
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)

            else:
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

甚至可以直接用bisect，找到位置...

## Conclusion

- 要学会利用之前掌握的东西，化零为整，完成一个更优的解法。
- 我尽量每道题目写出不止一种解法，但有的时候，真的是实力不济，无法短时间内写出来，而且锻炼逻辑能力，和算法能力，也并非一朝一夕的事情，大家一起慢慢来一点一点进步，相信复利。