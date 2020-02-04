# Leetcode No.29 Divide Two Integers

## Description

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
```

**Note:**

- Both dividend and divisor will be 32-bit signed integers.
- The divisor will never be 0.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.



## My Solution

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        return dividend/divisor
```

~~别打了，别打了，我知道错了。~~咳咳，言归正传，不要像上面瞎搞啊233，不然一个好端端的medium题目不是easier than easy 了吗。

首先先写最简单的，两个都是正数：

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        res = 0
        while dividend >= 0:
            res += 1
            dividend -= divisor
        return res-1
```

显然，存在负数参数的情况下，这个算法是失效的；在一正一负的情况下， `res += 1`也是一种南辕北辙的做法。

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        if dividend == 0: return 0
        res = 0
        tmp = 0
        if dividend < 0 and divisor < 0:
            while tmp > dividend:
                tmp += divisor
                res += 1
            return res
        elif dividend > 0 and divisor > 0:
            while tmp <= dividend:
                tmp += divisor
                res += 1
            return res - 1
        elif dividend > 0 and divisor < 0:
            while tmp <= dividend:
                tmp -= divisor
                res -= 1
            return res + 1
        else:
            while tmp > dividend:
                tmp -= divisor
                res -= 1
            return res
```

这样乍一看没什么问题，但是，不得不说的一个事情是，我没有接触过，`minus//minus`这种情况，或者说，当不使用`//`运算以后，我们才接触到了负数运算，所以不知道该向上取整还是向下取整。

其实这件事情应该做在写代码之前，就是要搞清楚需求。那么重新看一下：

```python
print(3//2)
print(-3//-2)
print(-3//2)
print(3//-2)

# Output
1
1
-2
-2

# 运算过程
3 = 2*1...1
-3 = -2*1...-1
-3 = 2*-2...1
3 = -2*-2...-1
```

看完我觉得...我傻了，这不就是单纯的向下取整吗，逻辑呢...看到这里，一下子就觉得，如果按照最开始那个`return dividend//divisor`就直接报错了啊...

## Reflection & Polishing-up

看了半小时以后，我决定不深究这道题了，因为我理不清，这个所谓除法的逻辑，一个确实是，python内置的整数除法，和题目中的除法的结果不同，而且题目中的example数量和情况的概括不全面，让我不能对这个算法有个比较完整的认知，所以我在挣扎了1个多小时以后就去看discussion了。

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        sig = 1 if dividend > 0 else -1
        dividend *= sig
        sig2 = 1 if divisor > 0 else -1
        divisor *= sig2
        
        if dividend < divisor:
            return 0
        
        q = 0
        while(dividend >= divisor):
            shift = 0
            tmp_divisor = divisor
            while(dividend >= (tmp_divisor << 1)):
                shift += 1
                tmp_divisor = (tmp_divisor << 1)
            dividend -= tmp_divisor
            q += (1<<shift)
        
        q = q*sig*sig2
        
        return max(-2**31, min(2**31-1, q))
```

我拿这个代码去试了一下，AC了，也就是说，在这个题目中，余数是可以为负数的。

也就是存在`-3 = -2 * 1 ..-1`的这种形式的，所以单纯当作正数除法来做从结果上就OK了。

另外学到了一点就是移位操作，这个是我之前没有想到的，之前确实一直困扰于：每次只能加减一个divisor的长度，导致时间太长，那么这里用移位操作，或者说单纯的除法就可以让这个操作的范围变大，从而减少消耗的时间。在这里`tmp_divisor << 1`可以等价于`tmp_divisor * 2`但是速度更快。

## Conclusion

最后，虽然是个简单的问题，但是，从某些方面来说也带来的很多启示：

- 在动手之前，先读懂需求。我觉得这一点在将来的职场生活中将会更加重要。
- 虽然我之前的思想是：不管是什么，先动起手来，把该做的先做了。但是通过这次教训，还是要先读懂需求，因为不管多好的Solution，解决不了问题就什么都不是。
- 可以用已知的的思路去解决当下的问题，比如这里的移位操作，或者单纯的乘2也比一个个加减要更有效率，这点在tcp网络的拥塞算法中也是有应用的。

这道题没做出来，总有种输掉了的感觉...好烦...