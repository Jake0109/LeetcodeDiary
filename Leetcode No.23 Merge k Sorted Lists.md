# Leetcode No.23 Merge k Sorted Lists

## Description

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

## My Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        res_l = head = ListNode(None)
        if not lists:
            return res_l.next
        for i in range(len(lists)):
            if not lists[i]:
                lists.pop(i)
                return self.mergeKLists(lists)

        dict = {}
        for i in range(len(lists)):
            dict[i] = lists[i].val
        while dict:
            m = min(dict.items(),key=lambda x:x[1])[0]
            res_l.next = lists[m]
            lists[m] = lists[m].next
            res_l = res_l.next
            if lists[m]:
                dict[m] = lists[m].val
            else:
                dict.pop(m)
        return head.next
```

我竟然独立完成了一个List的hard Problem...想起一个月以前坑坑洼洼地，还是借鉴别人才完成list的medium题目，感动...~~虽然慢的要死，指时间复杂度O(kn)~~

## Reflection & Polishing-up

官方的暴力破解：

```python
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        self.nodes = []
        head = point = ListNode(0)
        for l in lists:
            while l:
                self.nodes.append(l.val)
                l = l.next
        for x in sorted(self.nodes):
            point.next = ListNode(x)
            point = point.next
        return head.next
```

直接新建了一个list...真的很暴力呢...但感觉，比我这个蹩脚的还要高效？~~暴力破解不应该是效率最低的吗！你这个已经是O(nlogn)了啊！~~

然后我的解法就是这个：

#### Approach 2: Compare one by one

**Algorithm**

- Compare every \text{k}k nodes (head of every linked list) and get the node with the smallest value.
- Extend the final sorted linked list with the selected nodes.

官方甚至没有放代码，因为太低效了吗。。。

然后是这一种的改进版。

#### Approach 3: Optimize Approach 2 by Priority Queue

**Algorithm**

Almost the same as the one above but optimize the **comparison process** by **priority queue**. You can refer [here](https://en.wikipedia.org/wiki/Priority_queue) for more information about it.

```python
from Queue import PriorityQueue

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
```

没用过这个库...先看看吧。

## Conclusion

- 对于链表的问题，或者说对于所有的不擅长的问题，先动手，可能在不经意之间，通向Solution的大门就打开了。
- 链表的问题中，不一定非要执着于在已经构建好的链表之间操作，新建一个链表，可能会更省时间。