# Leetcode No.90 Subsets II

## Description

Given a collection of integers that might contain duplicates, ***nums\***, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

## My Solution

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        
        nums.sort()
        output = []
        
        for i in range(2**n,2**(n+1)):
            bitmask = bin(i)[3:]
            
            subset = [nums[j] for j in range(n) if bitmask[j] == '1']
            
            if subset not in output:
                output.append(subset)
        
        return output
```

这个没怎么调试过就AC了，但是好像效率不太高。用的是类似二进制的做法。

## Reflection & Polishing-up

```python
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums_dict={}
        for num in nums:
            nums_dict[num] = nums_dict.get(num,0)+1
        
        results = [[]]
        
        for num in nums:
            if nums_dict[num] > 0:
                temp = [ current+[num] for current in results]
                results.extend(temp)
                nums_dict[num] -= 1
                while nums_dict[num]>0:
                    temp = [ temp_current+[num]  for temp_current in temp]
                    results.extend(temp)
                    nums_dict[num] -= 1
        return results
```

一种迭代的思路，说实话，我也想要这么做做试试看的，但是想到二进制更符合我的思路，就先做二进制的Solution了。

```python
def subsetsWithDup(self, nums):
    res = []
    nums.sort()
    self.dfs(nums, 0, [], res)
    return res
    
def dfs(self, nums, index, path, res):
    res.append(path)
    for i in xrange(index, len(nums)):
        if i > index and nums[i] == nums[i-1]:
            continue
        self.dfs(nums, i+1, path+[nums[i]], res)
```

这个是我曾经最擅长的backtrack算法。由于有段时间没写回溯了，我也不晓得现在自己写回溯的效率怎么样...

## Conclusion

- 经由这次题目，主要是我又回去参考了之前的一些题目，发现了89题中可能的一个更优的解法。

```python
class Solution:
    def grayCode(self, n: int):

        bincode = "0b" + "0"*n
        res = []

        def changeCode(j):
            code = list(bincode)
            if code[j] == "1":
                code[j] = "0"
            else:
                code[j] = "1"
            return "".join(code)

        for i in range(2**n,2**(n+1)):
            temp = bin(i)
            binmask = temp[:2]+temp[3:]
            for j in range(len(binmask)-1,1,-1):
                # print(f"temp:{temp} bincode:{bincode}")
                if binmask[j] == "1":
                    bincode = changeCode(j)
                    res.append(int(bincode,2))
                    break
        return res
```

嗯，是一个小小的优化，但是从感官来说，我觉得很舒服。

- 进步，成长，这是我个人最看重的，想上面这样能够对过去的我有所提升的事情，大欢迎。