# Leetcode No.60 Permutation Sequence

## Description

The set `[1,2,3,...,*n*]` contains a total of *n*! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for *n* = 3:

1. `"123"`
2. `"132"`
3. `"213"`
4. `"231"`
5. `"312"`
6. `"321"`

Given *n* and *k*, return the *k*th permutation sequence.

**Note:**

- Given *n* will be between 1 and 9 inclusive.
- Given *k* will be between 1 and *n*! inclusive.

**Example 1:**

```
Input: n = 3, k = 3
Output: "213"
```

**Example 2:**

```
Input: n = 4, k = 9
Output: "2314"
```

## My Solution

首先是这个最原始粗略的：

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:

        res = []
        perm = []
        def permutate(count):
            if count == n:
                res.append(perm.copy())
            for i in range(1, n+1):
                if str(i) not in perm:
                    perm.append(str(i))
                    permutate(count + 1)
                    perm.remove(str(i))

        permutate(0)
        return "".join(res[k-1])
```

超时了，我思考了半天，发现了一个不用递归的方法：

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        mem = []
        left = [i for i in range(1, n + 1)]
        taken = []
        k = k - 1

        # to store !n
        for i in range(1,n):
            if not mem:
                mem.append(i)
                continue
            mem.append(i * mem[-1])

        if mem and k//mem[-1] == n:
            return "".join(reversed(left))

        while mem:
            temp = mem.pop()
            taken.append(str(left.pop(k//temp)))
            k = k % temp
        taken.append(str(left[0]))

        return "".join(taken)
```

## Reflection & Polishing-up

通过数据，我有这个自信：我的算法思路超过评论区的至少50%以上的人，什么回溯算法的我都不看了，唯一找到的一个跟我思路类似的是这个python的：

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        factorial_arr = []
        now_i = 1
        for i in range(1, 9):
            now_i *= i
            factorial_arr.append(now_i)
        st = ""
        now_arr = [i for i in range(1, n+1)]
        while True:
            if len(now_arr) == 0:
                break
            it = int((k-1) / factorial_arr[n-2])
            st += str(now_arr[it])
            n -= 1
            now_arr.pop(it)
            k -= it * factorial_arr[n-1]
        return st
```

当然，我是不会再继续一个个找的，效率太低。稍微讲解一些思路：

- 既然超时了，那就先确定最大位用什么值（可能就是所谓的剪枝，具体的没看过，不细说）
- 要确定最大位的话，拿9!举例，9! = 9 * 8!也就是说：
  - k // 8! +1 就是最大位数的值
- 发现逻辑的重复：
  - 我们可以建立一个list，假定为left，里面放的从1到n的排序；
  - 每一位得到需要的顺位以后，将该顺位pop出去，给结果列表taken；
  - 当left里面没有值的时候，停止循环；
- 用join将list转化成串返回。

这里解释一下，`str += str1 ... str += strn` 和 `list.append(str1)...list.append(strn) "".join(list)`两者各有优劣，在数据量小的时候，直接用 str 的加法快，但是当数量大的时候，join方法快。

## Conclusion

- 不破不立，只有当手上所有的武器全都不管用的时候，才能够掌握新的武器。
- 不要突破自己的上限，因为很危险。我们需要做的，是不断提高自己的上限。
- 也是日更了60个Leetcode笔记，也快超过3个月了，谈谈最近的生活。
  - 最近粉了一个人：沃伦·巴菲特，我是真的越来越觉得这个年过90的老爷子有意思，我这一生粉过两个人，一个是乔布斯，纯粹的崇拜，因为我过了几辈子估计都成不了乔布斯；另一个就是巴菲特，莫名的觉得，他对我而言有一种亲切感，即使我知道我对数字的敏感，以及选择的路都不一样，但是就会产生一种源于心底的好感。