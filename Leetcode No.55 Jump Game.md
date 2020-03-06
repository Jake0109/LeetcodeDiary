# Leetcode No.55 Jump Game

## Description

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

**Example 1:**

```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

## My Solution

我就说为什么前面做的那题是 Jump Game II呢，为啥1在2前面嘞？

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        goal = len(nums) - 1
        
        def trackback(index):
            if index >= goal:
                return True
            for i in range(nums[index], 0, -1):
                if trackback(index + i):
                    return True
            return False

        return trackback(0)
```

用回溯算法超时了...虽然我感觉应该是对的。那么就用比较数学的方法来看看吧。

首先我们来看一下，什么情况会发生不能到达终点呢：

- 当前的这个位置i的值nums[i]必须是0，且i<len(nums)-1，即终点位置。
- i 前面的任意一个位置 j ，它所指向的nums[j]+j <= i。
- 此时 i 之后的位置不可达，即无法到达终点。

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        count_of_zero = 0
        for i, num in enumerate(nums):
            if i == len(nums) - 1:
                return True
            if num == 0:
                if i == 0:
                    return False

                count_of_zero += 1
                for j in range(i - count_of_zero, -1, -1):
                    if j + nums[j] > i:
                        break
                else:
                    return False
            else:
                count_of_zero = 0
```

## Reflection & Polishing-up

看了一下官方的solution。

```java
public class Solution {
    public boolean canJumpFromPosition(int position, int[] nums) {
        if (position == nums.length - 1) {
            return true;
        }

        int furthestJump = Math.min(position + nums[position], nums.length - 1);
        for (int nextPosition = position + 1; nextPosition <= furthestJump; nextPosition++) {
            if (canJumpFromPosition(nextPosition, nums)) {
                return true;
            }
        }

        return false;
    }

    public boolean canJump(int[] nums) {
        return canJumpFromPosition(0, nums);
    }
}
```

不是，你这个回溯，跟我自己写的逻辑上有什么区别吗，你自己就能跑，我就超时？行吧...

这个在遍历的时候还是从小往大遍历

`for (int nextPosition = position + 1; nextPosition <= furthestJump; nextPosition++)`

我自己还特地从大往小遍历来着，哎呀...算了，我们看看别的solution吧。

有个dp的解法，我觉得挺有意思的，这个有点像那种图论里面是否可达的那种判断，我现在当然是不会写图论的题目的，以后慢慢刷题，研究研究，应该就能会了吧。

就是说为每一个index写标签，能够到达终点的是'good'反之就是'bad'，而我们需要做的就是考察0这个坐标是不是'good'。由于只看了一下逻辑，又没有python代码，我就自己写了一份。

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        good_index = [False for _ in range(len(nums))]
        good_index[-1] = True
        for i in range(len(nums) - 2, -1, -1):
            for j in range(nums[i], 0, -1):
                if i + j >= len(nums):
                    good_index[i] = True
                    break
                if good_index[i + j]:
                    good_index[i] = True
                    break

        return good_index[0]
```

然后又到了喜闻乐见的相同逻辑，java能通过，python3超时。我已经加了两个break来终止循环了...木得办法了。但是我写的逻辑应该是没有问题的。

最后是这个贪心算法，原来我看都不想看的，结果发现，~~真香警告~~思路好巧妙，就自己也写了一个。

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        last_pos = len(nums) - 1
        for i in range(len(nums) - 1, -1, -1):
            if i + nums[i] >= last_pos:
                last_pos = i

        if last_pos != 0:
            return False
        else:
            return True
```

## Conclusion

- 最近是真的被回溯束缚住的感觉，可能这也是我最近的思路吗？过于依赖回溯和递归，不是一件好事情，总之还是要多开拓思路吧。
- Python学习只是个起点，绝对不能是终点，可以看到相同的逻辑，python的运行效率是非常低的，以后如果需要对性能要求更高的框架，python肯定不会很适合，现在就先把目标定在Golang吧。