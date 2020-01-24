# Leetcode No.19 Remove Nth Node From End of List

## Description

Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

## My Solution

```python
#Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def removeNthFromEnd(self, head, n):
        size = 1
        cur = p = head
        while cur.next:
            size += 1
            cur = cur.next
            if size > n + 1:
                p = p.next
        if size == n:
            return head.next
        else:
            p.next = p.next.next
            return head
```

一个 one pass的Solution。今天身体不太好，就不写太多了

## Reflection & Polishing-up

去看了看别的Solution：

```python
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        tempb = ListNode(0)
        tempa, tempb.next, heada = head, head, tempb
        Indexa = 0
        while tempa:
            tempa = tempa.next
            Indexa += 1
            if Indexa > n:
                tempb = tempb.next
        tempb.next = tempb.next.next
        return heada.next
```

一个双节点的one pass Solution。