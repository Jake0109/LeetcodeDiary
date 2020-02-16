# Leetcode No. 39 Combination Sum

## Description

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

## My Solution

```python
class Solution:
    def combinationSum(self,candidates:list[int],target):
        candidates.sort()
        dict = {}
        res = []
        for candidate in candidates:
            if candidate > target:
                break
            pass

        return res
```

然后洋洋洒洒写了这么多...

```python
class Solution:
    def combinationSum(self,candidates,target):

        def count_elements(sums:dict):
            count = 0
            for elements in sums.values():
                for element in elements:
                    count += 1
            return count

        candidates.sort()
        sums = {}
        res = []
        for candidate in candidates:
            if candidate > target:
                break
            if candidate not in sums:
                sums[candidate] = set()
                sums[candidate].add((candidate,))
            else:
                sums[candidate].add((candidate,))

            current_len = count_elements(sums)
            while True:
                keys = list(sums.keys())
                for i in keys:
                    sum = i + candidate
                    if sum <= target:
                        for tmp_sums in sums[i]:
                            print(tmp_sums)
                            if sum in sums:
                                sums[sum].add(tmp_sums + (candidate,))
                            else:
                                sums[sum] = set()
                                sums[sum].add((tmp_sums + (candidate,)))

                if current_len == count_elements(sums):
                    break

                current_len = count_elements(sums)

        if not sums or target not in sums:
            return res
        for i in sums[target]:
            res.append(list(i))

        return res    
```

我真的认为，真的很复杂...暂时也没想到什么别的优秀解法出来...就很难受。

## Reflection & Polishing-up

在Discussion里面看到了，这一题可以用回溯来做，那，我们明天就用回溯来重新做这题吧！