# Leetcode No.56 Merge Intervals

## Description

Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## My Solution

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        
        if not intervals:
            return []
        
        # sorted the list by the start point.
        intervals = sorted(intervals, key=lambda x: x[0])

        # the numbers needed in result
        smallest = intervals[0][0]
        temp_largest = intervals[0][1]
        res = []

        index = 1
        while index < len(intervals):
            if temp_largest >= intervals[index][0]:
                temp_largest = max(temp_largest,intervals[index][1])
                index += 1
            else:
                res.append([smallest, temp_largest])
                smallest = intervals[index][0]
                temp_largest = intervals[index][1]
                index += 1
        # the last [start,end] is always not included
        res.append([smallest,temp_largest])

        return res
```

我觉得加上注释以后，代码应该已经比较好读了。就不过多解释了。

## Reflection & Polishing-up

看了一下官方给的解决方案，第一种是暴力破解，第二种就跟我的这个思路一样，这里把官方的代码给出来，写的非常优雅：

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:

        intervals.sort(key=lambda x: x[0])

        merged = []
        for interval in intervals:
            # if the list of merged intervals is empty or if the current
            # interval does not overlap with the previous, simply append it.
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
            # otherwise, there is overlap, so we merge the current and previous
            # intervals.
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

这个是那个暴力解法，看着说明应该是跟图论有关系的，很认真的看了一遍，没看懂，又看了一遍还是没看懂...就很烦。

```python
class Solution:
    def overlap(self, a, b):
        return a[0] <= b[1] and b[0] <= a[1]

    # generate graph where there is an undirected edge between intervals u
    # and v iff u and v overlap.
    def build_graph(self, intervals):
        graph = collections.defaultdict(list)

        for i, interval_i in enumerate(intervals):
            for j in range(i+1, len(intervals)):
                if self.overlap(interval_i, intervals[j]):
                    graph[tuple(interval_i)].append(intervals[j])
                    graph[tuple(intervals[j])].append(interval_i)

        return graph

    # merges all of the nodes in this connected component into one interval.
    def merge_nodes(self, nodes):
        min_start = min(node[0] for node in nodes)
        max_end = max(node[1] for node in nodes)
        return [min_start, max_end]

    # gets the connected components of the interval overlap graph.
    def get_components(self, graph, intervals):
        visited = set()
        comp_number = 0
        nodes_in_comp = collections.defaultdict(list)

        def mark_component_dfs(start):
            stack = [start]
            while stack:
                node = tuple(stack.pop())
                if node not in visited:
                    visited.add(node)
                    nodes_in_comp[comp_number].append(node)
                    stack.extend(graph[node])

        # mark all nodes in the same connected component with the same integer.
        for interval in intervals:
            if tuple(interval) not in visited:
                mark_component_dfs(interval)
                comp_number += 1

        return nodes_in_comp, comp_number

    
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        graph = self.build_graph(intervals)
        nodes_in_comp, number_of_comps = self.get_components(graph, intervals)

        # all intervals in each connected component must be merged.
        return [self.merge_nodes(nodes_in_comp[comp]) for comp in range(number_of_comps)]
```

## Conclusion

- 不知道为什么，写这个题目过程中觉得蛮爽的。可能最近在家里面憋久了，出什么问题了233。
- 有一种被大佬吊打的感觉啊：
  - 在写判断的时候，可以多考虑考虑条件应该如何取，比如这里官方的条件取的就比我自己的要好，cover的点要多。
  - 循环的时候，不一定每次都需要一个临时变量来记录状态，经验主义不可取。