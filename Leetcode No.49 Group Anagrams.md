# Leetcode No.49 Group Anagrams

## Description

Given an array of strings, group anagrams together.

**Example:**

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**

- All inputs will be in lowercase.
- The order of your output does not matter.

## My Solution

```python
from collections import defaultdict


class Solution:
    def groupAnagrams(self, strs):
        index = defaultdict(list)

        for str in strs:
            if not index:
                index[str].append(str)
                continue

            for key in index.keys():
                temp1 = list(str)
                temp2 = list(key)

                if not temp1:
                    index[str].append(str)
                    break
                elif not temp2:
                    continue

                for letter in temp2:
                    if letter in temp1:
                        temp1.remove(letter)
                    else:
                        # print("temp1:{} temp2:{} not valid".format(temp1,temp2))
                        break
                if not temp1:
                    # print("string:{} in key:{}".format(str,key))
                    index[key].append(str)
                    break
            else:
                # print("string:{} not valid".format(str))
                index[str].append(str)

        print(index)
        res = []
        for value in index.values():
            res.append(value)
        return res
```

写成这样，我也差不多了最后败在了一个压力测试上，我不太晓得，这个该怎么校验，如果说无所谓顺序的话。

它给我的程序输出也没给全，它自己的Expected也没给全，我上哪找去...但是感觉败在一个medium题目上好头疼...

## Reflection & Polishing-up

```python
class Solution(object):
    def groupAnagrams(self, strs):
        ans = collections.defaultdict(list)
        for s in strs:
            ans[tuple(sorted(s))].append(s)
        return ans.values()
```

我傻了，想到过用list但是因为list是可变的，所以没法当作key来使用，用set又会去重，忘了tuple，但是我自己用tuple也想不出这么简单的办法倒是了。

可恶啊~~无能狂怒ing~~

## Conclusion

- 对Python各个数据结构的使用和理解不到位，有待加强。
- 一个medium的题目做不出来是真的难受。