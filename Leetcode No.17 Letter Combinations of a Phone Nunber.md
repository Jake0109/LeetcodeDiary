# Leetcode No.17 Letter Combinations of a Phone Nunber

## Description
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

> 参考九键输入法，这里就不贴图了

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

## My Solution

试着用了一下DP思路，~~也不晓得是不是~~，但是感觉写了个很麻烦的代码...


```python
def letterCombinations(digits: str):
    dict = {"2": ["a", "b", "c"], "3": ["d", "e", "f"], "4": ["g", "h", "i"], "5": ["j", "k", "l"], "6": ["m", "n", "o"], "7": ["p", "q", "r", "s"], "8": ["t", "u", "v"], "9": ["w", "x", "y", "z"]}
    res = []

    def combine(base, addition):
        tmp = []
        if not base:
            return addition
        for i in base:
            for j in addition:
                tmp.append("".join([i, j]))
        return tmp

    for i in digits:
        res = combine(res, dict[i])
    return res
```

算下来，这个算法的时间复杂度应该是O(3^n)自然非常慢，但是...从我的看法而言，这样已经是不错的了，暂时想不到什么更好的solution。

## Reflection & Polishing-up

```python
class Solution:
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        def combine(pres,dicts,digit):
            """
            :type pres:List[str], dicts: dictionary, digits: str
            :rtype: List[str]
            """
            letL=[]
            for pre in pres:
                for letter in dicts[digit]:
                    letL.append(pre+letter)
            return letL
            
        if digits=='':
            return []
            
        dicts={'1':[''], '2':['a','b','c'],'3':['d','e','f'],'4':['g','h','i'],'5':['j','k','l'],'6':['m','n','o'],'7':['p','q','r','s'],'8':['t','u','v'],'9':['w','x','y','z'],'0':[' ']}
        
        lets=['']
        for digit in digits:
            lets=combine(lets,dicts,digit)
        return lets
```

学到两点：

- 当合并两个字符串的时候 + 操作比join()更快
- leetcode的最终结果是随机的，因为我用相同的代码试了两边，最后得出的runtime是不同的，有时候别纠结太多，主要看方法。

今天N2出成绩，161分，很开心~

