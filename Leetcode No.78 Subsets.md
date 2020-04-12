# Leetcode No.78 Subsets

## Description

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

## My Solution

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        length = len(nums)
        res = []

        for i in range(2 ** length):
            element = []
            for j, num in enumerate(nums):
                if i % 2 == 1:
                    element.append(num)
                i = i // 2
            print(element)
            res.append(element)

        return res
```

昨天写排列组合的时候，知道了一个类似二进制的方法，所以今天决定在这道题上来试一试。这需要感谢我的高中数学老师，那时候就教过我们一种计算集合子集的方法，就是每一个元素都可能在子集中也都有可能不在子集中，那就是两种状态，总共有2**n种可能，刚好也符合二进制的特点，就这么用了。

## Reflection & Polishing-up

看到这题我的直觉就在告诉我这题可以用回溯法做，看了一下讨论区就确信了。嗯...在进去看之前，自己可以试试写出来。

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        element = []

        def backtrack(i):
            if i == len(nums):
                res.append(element.copy())
                return
            backtrack(i+1)
            element.append(nums[i])
            backtrack(i+1)
            element.pop()
            # 也可以用element.remove(nums[i])


        backtrack(0)
        return res
```

ok，这下子两种主要的方法应该就齐活了。

但是看了一下官方给的解法，一下子落差就出来了...

官方的递归：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        output = [[]]
        
        for num in nums:
            output += [curr + [num] for curr in output]
        
        return output
```

官方的回溯我倒是有信心没他们写的差，但是那个二进制的解法就差的有点多：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        output = []
        
        for i in range(2**n, 2**(n + 1)):
            # generate bitmask, from 0..00 to 1..11
            bitmask = bin(i)[3:]
            
            # append subset corresponding to that bitmask
            output.append([nums[j] for j in range(n) if bitmask[j] == '1'])
        
        return output
```

差距来自与对于Python的理解不足。具体来说，就是我不知道bin()这个函数，可以直接将一个证书转化成一个二进制的str。~~不懂为什么是str的请使用type~~

## Conclusion

- 不知道不可怕，可怕的是不知道自己不知道。现在有一次体会到了这一点，通过这个想了一下这四年的大学生涯，春天里面吓得哆嗦。
- 还有一个，有想法就直接用，行动力，执行力，这一类东西是非常重要的。如果说恰好有人在看我的博客来写Leetcode，那么希望最好自己先思考，先写，写完再来看看博主我的思路是什么样的，能不能改进。
- 眼高手低不可取。