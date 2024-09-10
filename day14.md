# 代码随想录 Day14

## 226. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

[Course Link](https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

广度，深度遍历都行，这里我习惯用广度

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        from collections import deque
        if not root:
            return
        que = deque([root])
        while que:
            for _ in range(len(que)):
                node = que.pop()
                node.right, node.left = node.left, node.right
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
        return root
```

## 101. [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
[Course Link](https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html)
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        from collections import deque
        que = deque([root])
        while que:
            level = []
            for _ in range(len(que)):
                node = que.pop()
                if node:
                    level.append(node.val)
                    que.appendleft(node.left)
                    que.appendleft(node.right)
                else:
                    level.append(None)
            if level!=level[::-1]:
                return False
        return True
```
