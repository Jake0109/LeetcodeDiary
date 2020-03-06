# Leetcode No.50 Pow(x, n)

## Description

Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (xn).

**Example 1:**

```
Input: 2.00000, 10
Output: 1024.00000
```

**Example 2:**

```
Input: 2.10000, 3
Output: 9.26100
```

**Example 3:**

```
Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

**Note:**

- -100.0 < *x* < 100.0
- *n* is a 32-bit signed integer, within the range [−2^31, 2^31 − 1]

## My Solution

暴力解法就不介绍了，n为多少就循环多少次，大于0就乘，小于0就除。

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        elif n > 0:
            if n == 1:
                return x
            if n % 2:
                return self.myPow(x, n//2) * self.myPow(x, n//2) * x
            else:
                return self.myPow(x, n//2) * self.myPow(x, n//2)
        else:
            if n == -1:
                return 1/x
            if n % 2:
                return self.myPow(x, n//2) * self.myPow(x, n//2) * x
            else:
                return self.myPow(x, n//2) * self.myPow(x, n//2)
```

## Reflection & Polishing-up

明显上面的代码有~~写的挺烂的~~挺大的优化空间的。

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        elif n > 0:
            if n == 1:
                return x
            if n % 2:
                return self.myPow(x, n//2)**2*x
            else:
                return self.myPow(x, n//2)**2
        else:
            if n == -1:
                return 1/x
            if n % 2:
                return self.myPow(x, n//2) ** 2 * x
            else:
                return self.myPow(x, n//2) ** 2
```

二次方运算需要用两个递归函数太傻了。

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        else:
            if n == 1:
                return x
            if n == -1:
                return 1/x
            if n % 2:
                return self.myPow(x, n // 2) ** 2 * x
            else:
                return self.myPow(x, n // 2) ** 2
```

然后是同样逻辑块的整合。

这里必须注意一点的是，python中整除向下取整，所以在负数和正数的时候的操作相同，如果在负数的时候是向上取整的话，就不能这样了。

或者就是如果觉得在幂运算里面用`**`太傻了，那也可以这么做：

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        else:
            if n == 1:
                return x
            if n == -1:
                return 1/x
            square_root = self.myPow(x, n // 2)
            if n % 2:
                return square_root*square_root*x
            else:
                return square_root*square_root
```

我也暂时想不出什么优化的东西了，先去看看别人的吧：

```python
class Solution:
    def myPow1(self, x: float, n: int) -> float:
        
        myres = 1
        
        if n == 0: return 1
        elif n < 0: 
            x = 1/x
            n = -n
            
        curProd = x
        k = n
        
        while (k > 0):
            if k%2 == 1: myres *= curProd
            curProd *= curProd
            k //= 2
        return myres
```

少有的迭代方案，我觉得效率应该比我的这个高，不用每次递归的时候还要判断一遍正负，这个是非常好的。

```python
# Output
func:pow use time:9.700000000001374e-06
func:myPow1 use time:2.300000000003688e-06
```

这里我用pow包装了一下myPow()因为递归过程中，每次递归都会输出，很影响观感。高下立判，我输了orz。

## Conclusion

- 这次经历了一次比较完整的实现->优化的过程，感觉还可以。
- 可能之前不使用递归，导致现在开始有滥用递归的倾向。毕竟前段时间的笔试题里面，3题有两题我的想法都是回溯...