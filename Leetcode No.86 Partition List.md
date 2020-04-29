# Leetcode No.86 Partition List

## Description

Given a linked list and a value *x*, partition it such that all nodes less than *x* come before nodes greater than or equal to *x*.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**

```
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```

## My Solution

```python
class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        node = ListNode(None)
        node.next = head
        res = pre = node
        has_larger = False

        while head:
            if head.val < x:

                if not has_larger:
                    node = head
                    pre = pre.next
                    head = pre.next

                else:
                    pre.next = head.next
                    head.next = node.next
                    node.next = head

                    node = node.next
                    head = pre.next

            else:
                has_larger = True
                pre = pre.next
                head = pre.next

        return res.next
```

可以的话其实我不想引入`has_larger`这个变量的，但是我确实没有想到一个能够保证，不管有没有比x大的时候都能适用的流程。

## Reflection

我来看看，官方的怎么玩的：

```python
class Solution(object):
    def partition(self, head, x):
        # before and after are the two pointers used to create two list
        # before_head and after_head are used to save the heads of the two lists.
        # All of these are initialized with the dummy nodes created.
        before = before_head = ListNode(0)
        after = after_head = ListNode(0)

        while head:
            # If the original list node is lesser than the given x,
            # assign it to the before list.
            if head.val < x:
                before.next = head
                before = before.next
            else:
                # If the original list node is greater or equal to the given x,
                # assign it to the after list.
                after.next = head
                after = after.next

            # move ahead in the original list
            head = head.next

        # Last node of "after" list would also be ending node of the reformed list
        after.next = None
        # Once all the nodes are correctly assigned to the two lists,
        # combine them to form a single list which would be returned.
        before.next = after_head.next

        return before_head.next
```

## Conclusion

- 人家毕竟是官方，写的是真的好。同样的两个指针，比我的这个简洁很多，而且一致性也很高。