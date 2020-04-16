# Leetcode No.83 Remove Duplicates from Sorted List

## Description

Given a sorted linked list, delete all duplicates such that each element appear only *once*.

**Example 1:**

```
Input: 1->1->2
Output: 1->2
```

**Example 2:**

```
Input: 1->1->2->3->3
Output: 1->2->3
```

## My Solution

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        res = head
        while head and head.next:
            if head.val == head.next.val:
                head.next = head.next.next
            else:
                head = head.next
        
        return res
```

一次遍历的方法。

## Reflection & Polishing-up

看了一下，其实在讨论区大家的思路都大差不差，也就是递归和迭代两种做法，我个人是没想到递归该怎么做然后就去参观了一下：

```python
	def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        # remove one node and don't move the head pointer yet 
        if head.val == head.next.val:
            head.next = head.next.next
            self.deleteDuplicates(head)
        else: # not a duplicate search from next node
            head.next = self.deleteDuplicates(head.next)
        
        return head
```

还是能学到东西的。

## Conclusion

- 题目太简单突然不知道该说点什么...
- 或许应该把这题放到上一题的前面？好有个递进的，螺旋上升式的学习曲线？还是说让coder们体会一下打完boss清小怪的畅快感~~只要玩的不是黑魂~~？