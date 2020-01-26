# Leetcode No.11

## Description: Container With Most Water

Given *n* non-negative integers *a1*, *a2*, ..., *an* , where each represents a point at coordinate (*i*, *ai*). *n* vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and *n* is at least 2.

 

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

## My solution

```python
class Solution:
    def maxArea(self,height):
        res = 0
        for i in range(len(height)):
            for j in range(len(height)-1,i,-1):
               if (j-i)*min(height[i],height[j]) > res:
                   res = (j-i)*min(height[i],height[j])
        return res   
```

暴力解法，时间复杂度O(n^2)，显然是不合格的。于是我在想一个时间复杂度为O(n)的算法，可是回忆了一下以前作过的题目，也没想到什么能够运用的策略，所以我去看了看Official Solution里面的解决方案。里面一个优化的解法是Two Pointer Approach。我就以此为思路自己实现一遍试试。

```python
class Solution:
    def maxArea(self,height):
        res = 0
        start,end = 0,len(height)-1
        while end>start:
            if (end-start)*min(height[end],height[start]) > res:
                res = (end-start)*min(height[end],height[start])
            if height[start] > height[end]:
                end -= 1
            else:
                start += 1
        return res
```

确实这样的时间按复杂度就是O(n)，效率高多了。

## Reflection and Polishing-up

在看别人的solution的时候看到一个跟我类似的，我来分析一下：

```python
def maxArea(height):
	water = 0
    head = 0
    tail = len(height) - 1

    for cnt in range(len(height)):
        
        width = abs(head - tail)
        
        if height[head] < height[tail]:   
            res = width * height[head]
            head += 1
        else:
            res = width * height[tail]
            tail -= 1

        if res > water:
            water = res

    return water
```

我得出的结论是：

- 我的while循环理论上优于这个for循环：因为我的只需要循环n-1次，而这个for循环需要循环n次（多出来的一次是无用的head=tail的情况）
- 他的是比我的规范：[head,tail]对应该是比[start,end]更易于理解的。
- 他判断条件写的比我更优：我的每个循环内，需要做至少一次乘法和一次min()，如果需要修改返回值，还需要再执行一次乘法和一次min()，但是他这里每次都只需要一次乘法，不需要min()，即使他每次都要做一次abs()。

那么，我们要多看到别人的优点，缺点就注意自己不要犯，所以我优化了一下我的代码：

```python
def maxArea(height):
    volume = 0
    head,tail = 0,len(height)-1

    while tail>head:
        if height[tail] > height[head]:
            tmp = (tail-head)*height[head]
            head += 1
        else:
            tmp = (tail-head)*height[tail]
            tail -= 1

        if tmp > volume:
            volume = tmp
    return volume
```

事实证明，这样确实是有效的，之前只超过了66%的python3代码，这次直接超过了99%的代码。