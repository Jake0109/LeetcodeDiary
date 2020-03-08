# Leetcode No.57 Insert Interval

## Description



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

这个的条件判断有点复杂，看了一个贪心算法，没有看具体实现，稍微看了一下思路，还在摸索中。明天继续