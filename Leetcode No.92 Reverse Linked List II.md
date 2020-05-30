# Leetcode No.92 Reverse Linked List II

## Description

Reverse a linked list from position *m* to *n*. Do it in one-pass.

**Note:** 1 ≤ *m* ≤ *n* ≤ length of list.

**Example:**

```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

## My Solution

看到这题才发现...我好像没有试过反向链表的操作...

于是乎经历了坎坷，终于做完了：

```python
class Solution:
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:

        if n == m:
            return head

        i = 0
        startpoint = node = ListNode(None)
        node.next = head
        pre = post = None
        while node:

            if i == m - 1:
                pre = node

            if i == n + 1:
                post = node

            i += 1
            node = node.next

        p = pre
        q = pre.next
        while q != post:
            if p == pre:
                p = q
                q = q.next
                p.next = post
                continue

            temp = q.next
            q.next = p
            p = q
            q = temp

        pre.next = p

        return startpoint.next
```

## Reflection & Polishing-up

这里特别需要注意的一点是：用我的方法，不能返回head。因为当m为1的时候，head依然会在原先的第一个节点上，返回时会出错。所以我使用了一个探针，startpoint然后返回startpoint.next来避免这种问题。

官方给的Solution比我写的要好：

```python
class Solution:
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """

        # Empty list
        if not head:
            return None

        # Move the two pointers until they reach the proper starting point
        # in the list.
        cur, prev = head, None
        while m > 1:
            prev = cur
            cur = cur.next
            m, n = m - 1, n - 1

        # The two pointers that will fix the final connections.
        tail, con = cur, prev

        # Iteratively reverse the nodes until n becomes 0.
        while n:
            third = cur.next
            cur.next = prev
            prev = cur
            cur = third
            n -= 1

        # Adjust the final connections as explained in the algorithm
        if con:
            con.next = prev
        else:
            head = prev
        tail.next = cur
        return head
```

在找到起始点上就比我做的优秀，其实具体到思路是一样的。

## Conclusion

- 真的好久没有写链表的题目了~~，再加上好久没有做题了~~，明显感觉到生疏，当然也可能单纯是因为我在一个小时以前不会写链表的逆向...
- 持续性的刻意练习真的非常必要...