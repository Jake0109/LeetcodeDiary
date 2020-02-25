# Leetcode No.45 Jump Game II

## Description

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Example:**

```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Note:**

You can assume that you can always reach the last index.

## My Solution

```python
class Solution:
    def jump(self, nums) -> int:
        goal = len(nums) - 1
        index = 0
        res = 0

        while index < goal:
            res += 1
            length = nums[index]
            max_length_sum = 0
            max_sum_index = index + 1

            # current index can reach the goal
            if index + length >= goal:
                return res

            # find the point that the sum of index and num is the max.
            for i, num in enumerate(nums[index + 1:index + length + 1], start=1):
                if i + num > max_length_sum:
                    max_length_sum = i + num
                    max_sum_index = index + i
                if max_sum_index < goal and nums[max_sum_index] + max_sum_index >= goal:
                    return res + 1

            index = max_sum_index

        return res
```

嗯，也算是比较快AC的hard题目了...算是贪心算法吧。

## Reflection & Polishing-up

看了一下评论区的，感觉我这个，真的是贪心算法吗...

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        preMax, curMax = 0, 0
        jumps = 0
        for i in range(len(nums)-1):
            curMax = max(curMax, nums[i]+i)
            if i == preMax:
                jumps+=1
                preMax = curMax
        return jumps
```

感觉，完败...可读性，实现，都是完败...为什么人家写出来的这么简单啊orz。

觉得，我是贪心反被贪心误：

- 我认为没有必要遍历一整个nums列表，只需要没到一个位置的时候找到当前位置的下一个位置就可以了。所以没有用for循环而是用while。
- 为了确认下一个点，则需要一些临时变量存储信息。
- 但是while确实没有遍历整个列表，内部还是需要for循环来判断。

自己试了一下dp，也实现了一下，是这样的：

```python
class Solution:
	def jumpdp(self, nums):
        length = len(nums)
        cost = [len(nums) for _ in range(length)]
        cost[0] = 0

        for index, num in enumerate(nums):
            for j in range(1, nums[index] + 1):
                target_index = index + j
                if target_index > length - 1:
                    target_index = length - 1
                cost[target_index] = min(cost[target_index], cost[index] + 1)

                if target_index == length - 1:
                    break
        return cost[length - 1]
```

然后自己测了一下速：

```python
sol = Solution()
nums = [1, 1, 2]
print(sol.jump(nums), sol.jumpdp(nums))

# output
func:jump runtime:4.900000000002125e-06
func:jumpdp runtime:8.500000000001562e-06
2 2
```

明显慢了很多啊...

## Conclusion

- 明显感知到自己的弱小...继续加油吧。

