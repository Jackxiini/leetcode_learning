# 代码随想录 Day3

## 203. [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

[Course Link](https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html#%E6%80%9D%E8%B7%AF)

链表，注意要使用dummy_head，总体还是很简单的。

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        return dummy_head.next
```

## 707. [Design Linked List](https://leetcode.com/problems/design-linked-list/)

[Course Link](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html)

有什么想不通就拿 index = 0 做例子推演，注意：addAtIndex不用单独拿 if index==self.len 作为可能性去用addAtTail，因为 index 等于长度的时候，current 会落到最后一个 node，current.next 是 None，依旧是valid的，不用担心 current 会走到 None。

另外用到 index 的 function 都要注意考虑 index 小于 0 或者 大于等于链表 size 的情况 （除了 addAtIndex 可以等于）。

```
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.len = 0
        self.dummy_head = ListNode()

    def get(self, index: int) -> int:
        if index < 0 or index >= self.len:
            return -1
        current = self.dummy_head.next
        for _ in range(index):
            current = current.next
        return current.val

    def addAtHead(self, val: int) -> None:
        new = ListNode(val=val, next=self.dummy_head.next)
        self.dummy_head.next = new
        self.len += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy_head
        while current.next:
            current = current.next
        current.next = ListNode(val=val, next=None)
        self.len += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.len:
            return
        else:
            current = self.dummy_head
            for _ in range(index):
                current = current.next
            new = ListNode(val=val, next=current.next)
            current.next = new
            self.len+=1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.len:
            return
        if index < self.len:
            current = self.dummy_head
            for _ in range(index):
                current = current.next
            current.next = current.next.next
            self.len -= 1
```

## 206. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

[Course Link](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html)

有点绕脑子，建议画一画来理清思路。重点需要存储指向下一个 node 的指针（current.next），不然会被覆盖。还需要存一下当前的 node 作为前一个 node，因为需要反转 （current.next = pre）

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        current = head
        pre = None
        while current:
            tmp = current.next
            current.next = pre
            pre = current
            current = tmp
        return pre

```
