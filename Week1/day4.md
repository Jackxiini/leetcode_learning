# 代码随想录 Day4

## 24. [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

[Course Link](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

模拟一下，纸上画一画，把要做的步骤写下来，再去看看哪里会被覆盖的，用 tmp 调整一下

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        current = dummy_head.next
        pre = dummy_head
        while current and current.next:
            tmp = current.next.next
            current.next.next = current
            pre.next = current.next
            current.next = tmp
            pre = current
            current = current.next
        return dummy_head.next
```

## 19. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

[Course Link](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

用 dummy_head 可以避免很多特别情况，并且注意要删除倒数第 N 个，那就得处在倒数 N + 1 的节点上做动作。

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        count = 0
        dummy_head = ListNode(next=head)
        cur_fast = cur_slow = dummy_head
        while cur_fast.next:
            cur_fast = cur_fast.next
            count+=1
            if count >= n+1:
                cur_slow = cur_slow.next
        cur_slow.next = cur_slow.next.next
        return dummy_head.next
```

## 160. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

[Course Link](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E6%80%9D%E8%B7%AF)

对齐两个链表再找相交

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        lenA, lenB = 0, 0
        curA = headA
        curB = headB
        while curA:
            lenA += 1
            curA = curA.next
        while curB:
            lenB += 1
            curB = curB.next
        curA, curB = headA, headB
        if lenB > lenA:
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA
        diff = lenA - lenB
        for _ in range(diff):
            curA = curA.next
        while curA:
            if curA == curB:
                return curA
            curA = curA.next
            curB = curB.next
        return None
```

## 142. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

[Course Link](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)

具体参考 Course Link。其实自己模拟几个例子也能发现快慢指针相遇的 Node，到达 cycle 的起始 Node 的距离和 head 到起始 Node 的距离是一样的

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if fast == slow:
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        return None
```
