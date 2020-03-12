# Leetcode No.61 Rotate List

## Description

Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

**Example 1:**

```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**

```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

## My Solution

```python
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        
        if not head:
            return None
        
        # compute the length of List
        length = 1
        l = head
        while l.next:
            length += 1
            l = l.next

        # if k > length it will not work correctly
        # so wo use the remainder instead.
        k = k % length
        
        # if k=0, we do not need to change, and it will
        # not work correctly, either.
        if k == 0:
            return head

        # find the xth last node.
        p = q = head
        count = 0
        while count < k:
            p = p.next
            count += 1
        while p.next:
            p = p.next
            q = q.next
        
        # get the result
        res = q.next
        q.next = None
        p.next = head
        return res
```

我已经很清楚的用注释写出了我的思路了，这里就不多说了。

## Reflection & Polishing-up

看了一下，算法都大同小异，首先这肯定是比一个个把最后的一个节点接到头节点上是要更省时间的，因为：

- 一个个接：时间复杂度为O(k*n)
- 一次性接：时间复杂度为O(k)

孰优孰劣不言而喻。

## Conclusion

- 我还挺感谢这个顺序的，因为确实有一段时间没有做链表的题目了。
- 当初从一点链表都不会，到后来慢慢能自己写出链表的hard题目，花了我将近一周的时间。用进废退最简单的道理，如果一直不用，还不复习，肯定不太好。