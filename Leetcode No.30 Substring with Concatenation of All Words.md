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

首先呢，是再在我们普适的情况前面判断一次，相应的实现，我是这么写的：

```python
    count = {}
    for i in s:
        if i not in count:
            count[i]=1
        else:
            count[i] += 1
    Wcount = 0
    if len(count) == 1:
        for word in words:
            if word in s:
                Wcount += 1
    if Wcount == len(words):
        tmp = len(s) - len("".join(words))
        return [i for i in range(tmp+1)]
```

然而，就算加上了这一段，还是有问题...就是当出现`words = ['ab','ba','ba'],s = 'ababaab'`这种情况，虽然每个word的长度相等，但是没有任何人说，s也是按照这种长度来让人遍历的...说实话，到这里，我又准备重构代码了。

框架是这样：

```python
def findSubstring(s: str, words):
    if not s or not words:
        return []
    num = len(words)
    index = {}
    length = len(words[0])
    
    def update_index(i,index=index,s=s,length=length):
        pass
    
    res = []
    while -1 not in index.values():
        count = 0
        iMin = min(index.values())
        for i in index.values():
            if i in range(iMin,iMin+length*num,length):
                count += 1
        if count == num:
            res.append(iMin)
        update_index(iMin+1)
    return res
```

最终写出来是这样：

```python
def findSubstring(s: str, words):
    if not s or not words:
        return []
    num = len(words)
    length = len(words[0])

    def update_index(i,s=s,length=length):
        index = {}
        for j,word in enumerate(words):
            tmp = s.find(word,i)
            count = 0
            for k in index.values():
                if tmp not in range(k,k+length):
                    count += 1
            if count == len(index):
                index[j] = tmp
            else:
                index[j] = s.find(word,max(index.values())+length)
        return index


    index = update_index(0)
    res = []
    for loop in range(len(s)-length):
        print(index)
        count = 0
        iMin = min(index.values())
        for i in index.values():
            if i in range(iMin,iMin+length*num,length):
                count += 1
        if count == num:
            res.append(iMin)
        index = update_index(loop)
    return res
```

奈何，还是棋差一着，因为匹配的时候，是按照words的index顺序来匹配，而不是所谓的最优匹配（遍历s，来匹配）所以当其中最开始查找的index为1的时候，index是这样的：`{0: 2, 1: 1, 2: -1}`这显然与预期不符。

这道题着实花了我将近5个小时的时间，但是最终发现，我的算法，从根本上解决不了某一些数据，我也决定先将这个放下了。

## Reflection & Polishing-up

去看看别人怎么做的吧：

```python
class Solution:
    def findSubstring(self,s: str, words):
        if words == [] or s == '':
            return []

        word_length = len(words[0])

        words_dict = collections.defaultdict(int)

        for item in words:
            words_dict[item] += 1

        res = []

        for i in range(0,word_length):

            words_of_window = collections.defaultdict(int)
            num = 0

            start = i

            for j in range(i, len(s), word_length):
                word = s[j:j+word_length]

                if word in words_dict:

                    words_of_window[word] += 1
                    num += 1

                    if words_of_window[word] > words_dict[word]:
                        while s[start:start+word_length] != word:
                            words_of_window[s[start:start + word_length]] -= 1
                            start += word_length
                            num -= 1
                        words_of_window[word] -= 1
                        num -= 1
                        start += word_length
                    if num == len(words):
                        res.append(start)

                else:
                    num = 0
                    words_of_window.clear()
                    start = j + word_length

        return res
```

理解这段代码也花了半小时左右的时间，只能说，可能现在我的能力并不够写这道题目吧。当然花时间最长的是这一段：

```python
                    if words_of_window[word] > words_dict[word]:
                        while s[start:start+word_length] != word:
                            words_of_window[s[start:start + word_length]] -= 1
                            start += word_length
                            num -= 1
                        words_of_window[word] -= 1
                        num -= 1
                        start += word_length
```

这个是选取最优解的过程，或者说，是选取最近可能出现符合预期的结果的一个过程。比如`s="abbababaab" words = ['ab','ba','ba'],j=6`大家可以自己思考一下，这种情况下，这段代码是怎么工作的。

如果上面的情况能想清楚，自然就豁然开朗了。

```python
class Solution(object):
    def findSubstring(self, s, words):
        result =[]
        if not words or not s : return result
        n=len(words[0])
        words_total_len= len(words) * n      
        sl= len(s)
        if words_total_len > sl : return result
        words.sort()
        for i in range (sl-words_total_len+1):
            s1=s[i:i+words_total_len] #sliding window
            #create set  with words of same size
            s_temp = [s1[j:j+n] for j in range(0, len(s1), n)]
            s_temp.sort()                        
            if s_temp == words: result.append(i)                
        return(result)
```

相比上一个solution，这个读起来就很清晰了，也没有太需要理解力的代码段。看样子应该是要遍历s而不是word来解决这个问题啊...但是其实两者的实现思路应该都是这个sliding windows吧？这个滑动窗口机制看样子是非常有名的了，因为TCP/IP协议中的滑动窗口机制也是一个非常重要的特性。

## Conclusion

这个题目真的花了我很多的时间，从第一眼看上去觉得不是一个hard的题目，到最后焦头烂额仍然AC不能，确实体现了我看题目的时候，还是对未来预计不充分，或者说预估题目难度的能力还是不充分。说说收获吧：

- 首先，最近由于在看一些工程类的教程，发现了前一段时间写代码中，出现问题：主要是这个`for word in words:`看过之前我的日记的朋友们肯定知道，要是我之前写的话，肯定就直接`for i in words:`了，这个其实不是特别的好，在大型项目中，如果想要少写注释，那就需要更高的代码可读性。能够望文知意的变量和参数名就非常的重要。
- 然后就是，虽然这个题目，我重构了3次代码，但是在项目中，重构代码通常不是一个聪明的选择。重构代码需要的人力物力时间等开销都会非常大，当然我这个几十行的小代码，而且还是单一功能就另说。
- 最后说一点收获吧，在leetcode刷题，这也算是第30个题目了，虽然肯定花了不止一个月~~谁让你笨，几个hard题目要两天来消化~~，但是确实有明显的感觉逻辑比以前清晰了。链表也能写一写了，而不是以前看到怎么下手都不会。
- 另外发现，真正运用到实际生活中的很多东西，也是一些基础的代码，或者说思想，比如这题中的滑动窗口，再比如TCP拥塞中快启动的思想，再比如在遍历查找的时候用二分查找来节省时间...都是一些非常基础的思想。若是能运用到稍复杂代码中，甚至是生活中，相信“效率”肯定会与之前不一样。

年也过得差不多了，希望今年的自己能更好。