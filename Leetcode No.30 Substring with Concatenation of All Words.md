# Leetcode No.30 Substring with Concatenation of All Words

## Description

You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring(s) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

 

**Example 1:**

```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**

```
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```

## My Solution

Ver1.0：

```python
def findSubstring(s: str, words):
    index = {}
    length = len(words[0])
    for i,word in enumerate(words):
        index[i] = s.find(word)
    tmp_item = min(index.items(),key=lambda x : x[1])
    res = []
    while -1 not in index.keys():
        count = 0
        for i in index.keys():
            if i in range(tmp_item[0],tmp_item[0]+length*len(index),length):
                count +=1
        if count == len(index):
            res.append(tmp_item[0])
        index[tmp_item[0]] = s[tmp_item[0]+length:].find(words[tmp_item[0]])
        tmp_item = min(index.items(),key=lambda x : x[1])
    return res
```

这个是跑不出来的，hard的题目，整理逻辑用的，我重构一下。

```python
def findSubstring(s: str, words):
    if not s or not words:
        return []
    index = {}
    length = len(words[0])
    for i, word in enumerate(words):
        tmp = s.find(word)
        if tmp not in index.values():
            index[i] = s.find(word)
        else:
            index[i] = s.find(word, tmp+length)
    if -1 in index.values():
        return []
    res = []
    while -1 not in index.values():
        print(index)
        minItem = min(index.items(), key=lambda x: x[1])
        count = 0
        for i in range(len(index)):
            if index[i] in range(minItem[1], minItem[1] + length * len(index), length):
                count += 1
        if count == len(index):
            res.append(minItem[1])
        tmp = s.find(words[minItem[0]], minItem[1]+1)
        if tmp not in index.values():
            index[minItem[0]] = tmp
        else:
            index[minItem[0]] = s.find(words[minItem[0]],tmp+1)
    return res
```

我觉得这个方法应该已经可以解决大多数的情况了，除了这个：

```python
"aaaaaaaa"
["aa","aa","aa"]
```

这不是恶心人吗...只要有这条在，每一次循环我都要把整个字典的数据都更新一遍，这也太傻了吧...今天先到这里，明天继续。

