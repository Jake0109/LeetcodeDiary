# Leetcode No.21 Merge Two Sorted Lists

## Description

Share

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

## My Solution

链表是我弱项，让我自己慢慢倒腾倒腾。

```python
def mergeTwoLists(l1,l2):
    h1,h2 = l1,l2
    node = ListNode(None)
    while h1.next and h2.next:
        if h1.val > h2.val:
            node.next = h2
            h2 = h2.next
        else:
            node.next = h1
            h1 = h1.next
```

初版，问题很多，我慢慢改。

```python
def mergeTwoLists(l1: ListNode, l2: ListNode):
    head = node = ListNode(None)
    while l1 and l2:
        if l1.val > l2.val:
            node.next = l2
            l2 = l2.next
            node = node.next
        else:
            node.next = l1
            l1 = l1.next
            node = node.next

    if l1 or l2:
        if l1:
            node.next = l1
        else:
            node.next = l2
    head = head.next
    return head
```

有点像那种，合并两个有序数列。当然，别跟我说你的解法是`list3 = list1+list2 \n list3.sort()`，这个有点不上路子啊。

## Reflection & Polishing-up

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        head = l3 = ListNode(0)
        while(l1 is not None and l2 is not None):
            if l1.val <=l2.val:
                l3.next = ListNode(l1.val)
                l3 = l3.next
                l1 = l1.next
            else:
                l3.next = ListNode(l2.val)
                l3 = l3.next
                l2 = l2.next
        while l1:
            l3.next = ListNode(l1.val)
            l3 = l3.next
            l1 = l1.next
        while l2:
            l3.next = ListNode(l2.val)
            l3 = l3.next
            l2 = l2.next
        return head.next
```

...看他自己的描述，这个算法的runtime是24ms，可是，我看了一下...还没我自己写的好呢，然后我自己复制了一下这个代码去submit了一下...

Success

Runtime: 84 ms, faster than 6.57% of Python3 online submissions for Merge Two Sorted Lists.

Memory Usage: 12.7 MB, less than 100.00% of Python3 online submissions for Merge Two Sorted Lists.

算了...solution需要订阅，暂时还懒得订阅，去discussion里面也没看到太优秀的python代码，就不贴上来了。