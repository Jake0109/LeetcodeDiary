# Leetcode No.69 Sqrt(x)

## Description

Implement `int sqrt(int x)`.

Compute and return the square root of *x*, where *x* is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

**Example 1:**

```
Input: 4
Output: 2
```

**Example 2:**

```
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```

## My Solution

1.用现成函数的方法：

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        return int(sqrt(x))
```

2.自己写：

```python
class Solution:
    def mySqrt(self, x: int) -> int:

        start = 0
        end = x
        while start < end:
            mid = (start+end)//2
            sq = mid*mid
            if sq == x:
                return mid
            elif sq > x:
                end = mid-1
            else:
                start = mid + 1

        if start**2 > x:
            return start-1

        return start
```

二分法做的。

## Reflection & Polishing-up

看到一个能让人想起大一时光的Solution：

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        num = x
        while num * num > x:
            num = (num + x//num) // 2
        return num
```

牛顿迭代法（好像是这个名字）。有兴趣可以自行百度谷歌。

## Conclusion

- 还有有生疏的，比如二分法第一反应是迭代。
- 鬼晓得为什么，我就为了写一个二分竟然花了10分钟思考逻辑。