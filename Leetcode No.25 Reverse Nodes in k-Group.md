# Leetcode No.25 Reverse Nodes in k-Group

## Description

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.



**Example:**

Given this linked list: `1->2->3->4->5`

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.

## My Solution

这可难倒我了...单链表的反转...没写过啊...尽力而为吧...首先最简单的暴力解法。

```python
class Solution:
    def reverseKGroup(self,head: ListNode, k: int) -> ListNode:
        vals = []
        list = []
        while head:
            vals.append(head.val)
            head = head.next
        for i in range(0,len(vals),k):
            if i+k > len(vals):
                list.extend(vals[i:])
                break
            tmp = vals[i:i+k]
            list.extend(reversed(tmp))
        node = ListNode(0)
        head = node
        for i in list:
            node.next = ListNode(i)
            node = node.next
        return head.next
```

没什么技术含量，都是用list来做的。然后老老实实在原有链表上操作吧。这个让我慢慢做，好歹也是个hard的硬骨头。首先是这样：

```python
def reverseKGroup(head: ListNode, k: int) -> ListNode:
    if k == 1 or not head:
        return head
    node = head
    while node:
        pass
    	node = node.next
    return head
```

然后把需要reverse的次数记录下来。

```python
def reverseKGroup(head: ListNode, k: int) -> ListNode:
    if k == 1 or not head:
        return head
    node = head
    pos_nodes = []
    count = 1
    while node:
        if count == k:
            pos_nodes.append(node)
            count = 0
        count += 1
        node = node.next
        head = pos_nodes[0]
    for i in range(len(pos_nodes)):
        pass
    return head
```

开始reverse：

```python
class Solution:
    def reverseKGroup(self,head: ListNode, k: int) -> ListNode:
        if k == 1 or not head:
            return head
        node = head
        pos_nodes = []
        count = 1
        while node:
            if count == k:
                pos_nodes.append(node)
                count = 0
            count += 1
            node = node.next
        p = head
        Prev = prev = None
        for i in range(len(pos_nodes)):
            if not Prev:
                head = pos_nodes[i]
                Prev = prev = p
                p = p.next
                Prev.next = pos_nodes[i].next
            elif Prev == prev:
                Prev.next = pos_nodes[i]
                Prev = prev = p
                p = p.next
                Prev.next = pos_nodes[i].next
            while p != pos_nodes[i]:
                tmp = p.next
                p.next = prev
                prev = p
                p = tmp
            else:
                tmp = p.next
                p.next = prev
                prev = p
                p = tmp
        return head
```

目前写成这样已经花了快3个小时了，虽然没有正确写出来。~~等等我不是暴力解法做出来了吗~~。手上还有点别的事情要做，~~就是你懒了~~，我们明天继续。

大家早上好，今天总算是把这个做完了。没有用上面的，直接重新按照思路写了一遍。

```python
class Solution:
    def reverseKGroup(self,head: ListNode,k:int):
        pos_nodes = []
        count = 1
        tmp_node = head
        while tmp_node:
            if count == k:
                pos_nodes.append(tmp_node)
                count = 0
            count += 1
            tmp_node = tmp_node.next
        Pre = pre = None
        p = head
        for i in range(len(pos_nodes)):
            if not Pre:
                head = pos_nodes[i]
                Pre = pre = p
                p = p.next
                Pre.next = pos_nodes[i].next
            else:
                Pre.next = pos_nodes[i]
                Pre = pre = p
                p = p.next
                Pre.next = pos_nodes[i].next
            for j in range(k-1):
                tmp = p.next
                p.next = pre
                pre = p
                p = tmp
        return head
        
```

那么我觉得，这个还可以重构一下子：

```python
class Solution:
    def reverseKGroup(self,head: ListNode,k:int):
        pos_nodes = []
        count = 1
        tmp_node = head
        while tmp_node:
            if count == k:
                pos_nodes.append(tmp_node)
                count = 0
            count += 1
            tmp_node = tmp_node.next
        Pre = pre = None
        p = head
        for i in range(len(pos_nodes)):
            if not Pre:
                head = pos_nodes[i]
            else:
                Pre.next = pos_nodes[i]
            Pre = pre = p
            p = p.next
            Pre.next = pos_nodes[i].next
            for j in range(k-1):
                tmp = p.next
                p.next = pre
                pre = p
                p = tmp
        return head
```

嗯，更简洁了，总算是把这个做好了。

## Reflection & Polishing-up

这个hard的链表题比上一个难多了...~~就是你太菜~~总之花了我两天的时间，虽然昨天已经暴力解出来了，但是，我做这套题目的目的就是为了增加对数据结构和算法的理解，通过实践增长知识，如果一味想着“逃课”那就没有意义了，有这个时间看看日剧它不香吗（笑。这个没有Official Solution，discussion区里面的python大多没有我的代码快，这就不贴出来了。

