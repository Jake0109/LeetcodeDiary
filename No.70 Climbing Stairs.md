# No.70 Climbing Stairs

## Description

You are climbing a stair case. It takes *n* steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Note:** Given *n* will be a positive integer.

**Example 1:**

```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2:**

```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## My Solution

提到费波纳提数列，那可是最经典的递归案例啊。至于为什么爬楼梯是费波纳提数列，请自行思考。

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n < 3:
            return n
        else:
            return self.climbStairs(n-1) + self.climbStairs(n-2)
```

但是，普通的递归是不会比得上迭代的。

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        
        if n < 3:
            return n
        
        memo = [1,2]
        for i in range(n-2):
            memo.append(memo[-1] + memo[-2])
        return memo[-1]
```

为什么，因为，在递归运行完climbStairs(n-1)以后，还需要从头再推演一次climbStairs(n-2)，但是在运行climbStairs(n-1)的时候，已经把climbStairs(n-2)算出来了，多花的时间就是在这里。

但是，是有办法让递归也变得高效的：

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        memo = [1,2]
        
        def rec(n):
            
            if n > len(memo):
                memo.append(rec(n-1) + rec(n-2))
            
            return memo[n-1]
        
        return rec(n)
```

也就是加缓存。

## Reflection & Polishing-up

在刚刚的迭代，或者说DP和带缓存的迭代中，使用了一个list存储暂时的量，从而达到高效的目的，能不能不用额外的内存呢？

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n < 3:
            return n
        
        start = 1
        end = 2
        for n in range(n-2):
            end, start = end+start, end
        
        return end
```

只需要两个变量就可以完成费波纳提数列取第n个值得操作了。

另外看到了一个神操作：

也就是官方的Approach 6：得到了一个费波纳提数列的公式f(n)

```java
public class Solution {
    public int climbStairs(int n) {
        double sqrt5=Math.sqrt(5);
        double fibn=Math.pow((1+sqrt5)/2,n+1)-Math.pow((1-sqrt5)/2,n+1);
        return (int)(fibn/sqrt5);
    }
}
```

这我就不自己实现了，数学公式。有点厉害的。

## Conclusion

- 像费波纳提这么一个简单的东西都可以拓展出这么多，没有理由在AC以后就不去思考一道题。
- 或许这个问题还有更多值得思考的空间，大家可以试试。