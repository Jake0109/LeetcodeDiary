# Leetcode No.24 Swap Nodes in Pairs

## Description

Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

 

**Example:**

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

## My Solution

Ver 1.0：

```python
def swapPairs(head: ListNode) -> ListNode:
    if not head or not head.next:
        return head
    p = head
    head = head.next
    while p.next.next:
        if not p.next:
            break
        q = p.next
        p.next = q.next
        q.next = p
        p = p.next
    return head
```

 啊...不行，这个问题太多，重新写一下。

```python
class Solution:
    def swapPairs(self,head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        p, q = head, head.next
        head = head.next
        while p.next:
            if not q.next:#当前的p,q节点处理完就结束的情况
                q.next = p
                p.next = None
                break
            elif not q.next.next:#当前位置看过去，还剩3个node没有遍历
                p.next = q.next
                q.next = p
                break
            else:#最普遍的情况
                p.next = q.next.next
                tmp = q.next
                q.next = p
                p = tmp
                q = p.next
        return head
```

这样就好多了，分为三种情况，我已经在代码块中做了注释，代码的实现并不难，~~但是花了很多时间，大概就是我菜吧~~所以就不多赘述了。

## Reflection & Polishing-up

Official Solutions需要订阅解锁...所以就去Discussion区看看吧。

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        prev, node = None, head
        while node and node.next:
            nodeNext = node.next.next
            node.next.next = node
            if prev:
                prev.next = node.next
            else:
                head = node.next
            prev = node
            node.next = nodeNext
            node = nodeNext
        return head
```

更普适，更简洁的代码，用`prev`来解决当前一个pair解决以后，下一个pair的顺序问题，是个没有想到的思路。

原来以为我的runtime已经超过了97%的python3代码，已经非常优秀了，没想到还有这么简洁的，学习了。这种代码的简洁性，或者说这一类的解决问题的思路，确实不是一朝一夕就能练成的，路还长啊...