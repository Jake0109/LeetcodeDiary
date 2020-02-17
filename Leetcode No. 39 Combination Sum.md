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

在Discussion里面看到了，这一题可以用回溯来做，那，我们明天就用回溯来重新做这题吧！今天早上上了学校的直播课...没做了，看了看别人写的。

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        
        
        
        candidates.sort()
        result = []
        print(candidates)
        
        def findCombinator(candidates,target,temp):
 
            for item in candidates:
                if(item > target):
                    return
                temp.append(item)
                if(item == target):
                    result.append(temp.copy())
                    temp.pop()
                else:
                    index = candidates.index(item)
                    findCombinator(candidates[index:],target-item,temp)
                    temp.pop()
                    
        findCombinator(candidates,target,[])
        
        return result
```

之前应该也提到了，最近在看代码规范，而且也决定身体力行，那么我就根据自己的理解，稍微改一改这个代码吧：

```python
class Solution:    
    def combinationSum1(self,candidates,target):
        candidates.sort()
        result = []

        def findCombinator(candidates, target, temp_result_list):
            for item in candidates:
                if item > target:
                    return
                temp_result_list.append(item)
                if item == target:
                    result.append(temp_result_list.copy())
                    temp_result_list.pop()
                else:
                    index = candidates.index(item)
                    findCombinator(candidates[index:], target - item, temp_result_list)
                    temp_result_list.pop()

        findCombinator(candidates, target, [])
```

嗯...只是排版，和一个变量的名命吧。

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        t_list = [[] for _ in range(target)]            
        sorted(candidates)                              #O(nlogn)
         
		#iterate 1 through target number
        for n in range(1, target+1):  
            for adder in candidates:               
                if n-adder < 0:
                    continue
                elif n-adder == 0:                  
                    t_list[n-1].append([adder])
                
                else:
                    for comb in t_list[n-adder-1]:                       
                        if not comb: continue   #if none pass
                        else:
                            if comb[-1] <= adder:                            
                                t_list[n-1].append(comb + [adder])
        return t_list[-1]
```

另外也看到了一个DP的解法（链接是这个：https://leetcode.com/problems/combination-sum/discuss/508334/python3-DP-iterative-solution）这里由于排版的原因，稍微删减了一点注释，但是原作者的注释我觉得写的挺方便理解这个算法的，有兴趣可以去看看。

## Conclusion

- 学会从更多的角度看待问题，比如看代码的时候就可以多从工程角度，代码规范的角度看。这样的话，每一次名命变量的时候，也能多一点思考，保证以后自己看的时候能轻松一点。
- 以后要学着写注释，虽然我非常不想这么做...~~爬~~
- 经历了两次回溯算法以后，总结了一点：
  - 回溯算法本质是递归
  - 在调用当前算法之后，需要一个回溯的操作。消除本次函数对于结果集的影响。