# Leetcode No.68. Text Justification

## Description

Given an array of words and a width *maxWidth*, format the text such that each line has exactly *maxWidth* characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly *maxWidth* characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no **extra** space is inserted between words.

**Note:**

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than 0 and not exceed *maxWidth*.
- The input array `words` contains at least one word.

**Example 1:**

```
Input:
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**Example 2:**

```
Input:
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be",
             because the last line must be left-justified instead of fully-justified.
             Note that the second line is also left-justified becase it contains only one word.
```

**Example 3:**

```
Input:
words = ["Science","is","what","we","understand","well","enough","to","explain",
         "to","a","computer.","Art","is","everything","else","we","do"]
maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

## My Solution

```python
class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:

        # generate the string needed to be added.
        def generate_string(start, end, length):

            # if only one word passed
            if start == end:
                return words[start] + " " * (maxWidth - length)

            string = words[start]
            int_len = (maxWidth - length) // (end - start)
            left = (maxWidth - length) % (end - start)

            # if it is the last string to be generated
            if end == len(words) - 1:
                for word in words[start + 1:end + 1]:
                    string += " " + word
                string += " " * (maxWidth - length)
                return string

            for word in words[start + 1:end + 1]:
                string += " " * (int_len + 2) if left > 0 else " " * (int_len + 1)
                left -= 1
                string += word

            return string

        res = []
        start = 0
        end = 0
        length = 0
        for i, word in enumerate(words):
            if i == 0:
                length = len(words[0])
                continue

            if len(word) + length + 1 > maxWidth:
                res.append(generate_string(start, end, length))
                start = end = i
                length = len(word)

            else:
                length = length + len(word) + 1
                end = i

        res.append(generate_string(start, end, length))

        return res
```

注释已经给了，我觉得虽然是一道hard题目，但是总体上逻辑不是很麻烦。

## Reflection & Polishing-up

去评论区看看，看到了一个16ms反观自己的28ms，我就进去了。

```python
class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        
        def construct(line,ans,maxWidth):
            '''
            line is an array of words to be padded, this function
            calculates the padding in between each word and fit them together
            '''
            if len(line) == 1:
                while len(line[0]) < maxWidth:
                    line[0] += " "
                ans.append(line[0])
                return
            pads, sentence = maxWidth, ""
            for w in line:
                pads -= len(w)
            # for n words, there's n-1 spacing, total number of spacing
            # should be 'pads', each spacing has pads//(n-1) spaces
            # extra spaces goes to first pads%(n-1) spacings.
            min_space = " "*(pads//(len(line)-1))
            k = pads%(len(line)-1)
            for i in range(len(line)-1):
                sentence += line[i] + min_space
                if k > 0:
                    sentence += " "
                    k -= 1
            sentence += line[-1]
            ans.append(sentence)
            return
        
        n, i, tot_len, line, ans = len(words), 0, -1, [], []
        while i < n:
            while i < n and tot_len <= maxWidth:
                tot_len += len(words[i])+1
                line.append(words[i])
                i += 1
            # i hits n and tot_len not full, we found last line. The last line is 
            # found only when i == n and tot_len <= maxWidth, all other cases
            # you haven't found the last line and tot_len > maxWidth caused the
            # while loop to break
            if i == n and tot_len <= maxWidth:
                sentence = ""
                for w in line:
                    sentence += w + " "
                while len(sentence) < maxWidth:
                    sentence += " "
                while len(sentence) > maxWidth:
                    sentence = sentence[:-1]
                ans.append(sentence)
            else:
                i -= 1
                tot_len -= (len(words[i])+1)
                line.pop()
                construct(line,ans,maxWidth)
                tot_len, line = -1, []
            
        return ans
```

总体思路是一样的，我也没看出什么太先进的东西，于是自己用这个代码去跑了一下，也是28ms...“标题党”无处不在，但是名不副实的吹牛就不太好了吧？

## Conclusion

- 毕业设计的后端做完了，也就继续慢慢研究leetcode的这些个题目了。准备写个毕业设计项目的后端总结，不晓得今天能不能写完。
- 上来就是个hard题目，却意想不到的一小时以内AC了，可喜可贺，可喜可贺。
- 在做项目中开始去阅读源码了以后，现在看浙西额代码，还是比较轻松加愉悦的，大家自己也可以去试试。