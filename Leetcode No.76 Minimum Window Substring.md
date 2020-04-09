# Leetcode No.76 Minimum Window Substring

## Description

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

**Example:**

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

**Note:**

- If there is no such window in S that covers all characters in T, return the empty string `""`.
- If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

## My Solution

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        count = {}
        for item in t:
            if item not in count:
                count[item] = 1
            else:
                count[item] += 1

        required = len(count)
        satisfied = 0
        window_count = {}

        left = 0
        right = 0

        ans = len(s)+1,None,None

        while right < len(s):
            char = s[right]
            if char not in window_count:
                window_count[char] = 1
            else:
                window_count[char] += 1

            if char in count and window_count[char] == count[char]:
                satisfied += 1

            while left <= right and satisfied == required:
                char = s[left]

                if right - left < ans[0]:
                    ans = right-left,left,right

                window_count[char] -= 1

                if char in count and window_count[char] < count[char]:
                    satisfied -= 1

                left += 1

            right += 1

        return "" if ans[0] == len(s)+1 else s[ans[1]:ans[2]+1]
```

嗯，这个是看了一遍官方的solution以后写出来的。

## Reflection & Polishing-up

这个是我借鉴的：

```python
def minWindow(self, s, t):
    """
    :type s: str
    :type t: str
    :rtype: str
    """

    if not t or not s:
        return ""

    # Dictionary which keeps a count of all the unique characters in t.
    dict_t = Counter(t)

    # Number of unique characters in t, which need to be present in the desired window.
    required = len(dict_t)

    # left and right pointer
    l, r = 0, 0

    # formed is used to keep track of how many unique characters in t are present in the current window in its desired frequency.
    # e.g. if t is "AABC" then the window must have two A's, one B and one C. Thus formed would be = 3 when all these conditions are met.
    formed = 0

    # Dictionary which keeps a count of all the unique characters in the current window.
    window_counts = {}

    # ans tuple of the form (window length, left, right)
    ans = float("inf"), None, None

    while r < len(s):

        # Add one character from the right to the window
        character = s[r]
        window_counts[character] = window_counts.get(character, 0) + 1

        # If the frequency of the current character added equals to the desired count in t then increment the formed count by 1.
        if character in dict_t and window_counts[character] == dict_t[character]:
            formed += 1

        # Try and contract the window till the point where it ceases to be 'desirable'.
        while l <= r and formed == required:
            character = s[l]

            # Save the smallest window until now.
            if r - l + 1 < ans[0]:
                ans = (r - l + 1, l, r)

            # The character at the position pointed by the `left` pointer is no longer a part of the window.
            window_counts[character] -= 1
            if character in dict_t and window_counts[character] < dict_t[character]:
                formed -= 1

            # Move the left pointer ahead, this would help to look for a new window.
            l += 1    

        # Keep expanding the window once we are done contracting.
        r += 1    
    return "" if ans[0] == float("inf") else s[ans[1] : ans[2] + 1]
```

还有一个优化版本：

```python
def minWindow(self, s, t):
    """
    :type s: str
    :type t: str
    :rtype: str
    """
    if not t or not s:
        return ""

    dict_t = Counter(t)

    required = len(dict_t)

    # Filter all the characters from s into a new list along with their index.
    # The filtering criteria is that the character should be present in t.
    filtered_s = []
    for i, char in enumerate(s):
        if char in dict_t:
            filtered_s.append((i, char))

    l, r = 0, 0
    formed = 0
    window_counts = {}

    ans = float("inf"), None, None

    # Look for the characters only in the filtered list instead of entire s. This helps to reduce our search.
    # Hence, we follow the sliding window approach on as small list.
    while r < len(filtered_s):
        character = filtered_s[r][1]
        window_counts[character] = window_counts.get(character, 0) + 1

        if window_counts[character] == dict_t[character]:
            formed += 1

        # If the current window has all the characters in desired frequencies i.e. t is present in the window
        while l <= r and formed == required:
            character = filtered_s[l][1]

            # Save the smallest window until now.
            end = filtered_s[r][0]
            start = filtered_s[l][0]
            if end - start + 1 < ans[0]:
                ans = (end - start + 1, start, end)

            window_counts[character] -= 1
            if window_counts[character] < dict_t[character]:
                formed -= 1
            l += 1    

        r += 1    
    return "" if ans[0] == float("inf") else s[ans[1] : ans[2] + 1]
```

这里就是将遍历的对象改编成了filter而不是s，filter就是将t中的元素筛选出来以后的(index,char)集合，当s中存在大量不属于t中的元素时会比上一个更加有效率。

最开始我的思路其实也是这样的，只不过我是用了字典的方法，将元素的位置存储起来，然后来判断。当然这样的问题就是，当t中相同元素存在多个，此时因为字典的互异性，会出现问题，所以没有办法自行解决。

## Conclusion

- 一方面最近生病刚好，确实不太清醒，所以写这些稍微难一些的题目确实挺吃力的。
- 另一方面：这道题其实可以拆解成两个步骤：
  - 字符串包含问题，即一个字符串是否是另一个字符串的子集。
  - 滑动窗口确定最小值。
- 这其中的第一点，我印象里面也是leetcode的一道题，之前就没做出来，那我希望做完这题以后，碰到这样的题目可以很轻松的做出来。
- 另外昨天写到一半家里面断电了，Typora好像并没有断电保护的机制，所以，哎...