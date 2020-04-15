# Leetcode No.82 Remove Duplicates from Sorted List II

## Description

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.

Return the linked list sorted as well.

**Example 1:**

```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```

**Example 2:**

```
Input: 1->1->1->2->3
Output: 2->3
```

## My Solution

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        
        if not head:
            return head
        
        node = head

        duplicates = set()
        while node.next:
            if node.next.val == node.val:
                duplicates.add(node.val)
            node = node.next
        
        print(duplicates)
        node = head
        head = ListNode(None)
        head.next = node
        left = head
        while node:
            if node.val in duplicates:
                left.next = node.next
            else:
                left = left.next
            node = node.next
        
        return head.next
```

嗯...遍历了两次，而且还要用集合记录状态。可以的话我还是希望能写出一个遍历一次的Solution。

## Reflection & Polishing-up

我自己能写出来的差不多到这里：

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:

        if not head:
            return head

        node = head
        head = ListNode(None)
        left = head
        head.next = node

        cur_dup = False
        while node.next:
            if node.val == node.next.val:
                node.next = node.next.next
                cur_dup = True
            else:
                if cur_dup:
                    left.next = node.next
                    node = node.next
                else:
                    left.next = node
                    left = left.next
                    node = node.next
                cur_dup = False

        return head.next
```

这个方法的弊端是需要去验证一个`node.val == node.next.val`但是如果是像`[1,1,2,2,2]`这样的数据的话，就会出现最后一个`2`后面没有节点比较，导致数据失真。

我还是看看别人怎么写的吧

```python
class Solution():
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        save_head = ListNode(0)
        save_head.next = head
        prev = save_head
        foundDuplicates = False
        
        while head and head.next:
            while head and head.next and head.val == head.next.val:
                foundDuplicates = True
                head = head.next
            if foundDuplicates:
                prev.next = head.next
                foundDuplicates = False
            else:
                prev = head
            head = head.next
            
        return save_head.next
```

个人见解：如果一次遍历来实现的话必定是要使用双指针的，这里用了两次循环找到了duplicate的结束位置，这个是我比较避讳但是很好用的一个方法。~~众所周知，嵌套循环不一定会增加时间复杂度~~

## Conclusion

- 我选择了用空间换取了时间，~~但实际上并没有~~总归需要一些思路来解决问题。
- 我的观点一直是这样的，先通过，再优化。