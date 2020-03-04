# Leetcode No.53 Maximum Subarray

## Description

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Follow up:**

If you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.

## My Solution

```python
class Solution:
    def maxSubArray(self, nums):
        dp = []

       	# to find maximum end with 'num'
        for i, num in enumerate(nums):
            if i == 0:
                dp.append(nums[0])
                continue
            dp.append(max(dp[i - 1] + num, num))
        return max(dp)
```

想不出来...怎么用分治法解这道题...

## Reflection & Polishing-up

参考了一下别人写的，自己试着写了一下：

```python
class Solution:
    def maxSubArray(self, nums):
        def divide_conquer(left, right):
            if left == right:
                return nums[left]

            middle = (left + right) // 2

            cross_sum = 0
            left_submax = nums[middle]
            right_submax = nums[middle + 1]
            curr_submax = 0
            for i in range(middle, left - 1, -1):
                curr_submax += nums[i]
                left_submax = max(left_submax, curr_submax)

            curr_submax = 0
            for i in range(middle + 1, right + 1):
                curr_submax += nums[i]
                right_submax = max(right_submax, curr_submax)

            cross_sum += left_submax + right_submax
            left_sum = divide_conquer(left, middle)
            right_sum = divide_conquer(middle + 1, right)
            # print("range: {} left_sum: {} right_sum: {} corss_sum: {} left_submax: {} right_submax: {}".format(
                [left, right], left_sum, right_sum, cross_sum, left_submax, right_submax))
            return max(cross_sum, left_sum, right_sum)

        return divide_conquer(0, len(nums) - 1)
```

有一段是测试用的，大家看看就好，个人觉得分治法是比较繁琐的一个解法吧...而且感觉其实耗时也不短...但是这段纠结的时间应该，或者说我希望对我将来有用处。

## Conclusion

- 相当于复习了一遍分治法，说是复习，我自己之前也就写过两个分治法的东西，一个是算法考试在纸上写的不知道对不对的，另外一个就是快排。这次算是一个查漏补缺性质的东西。
- 这个easy的题目，愣是让我搞出了hard的感觉，真的是...一言难尽。