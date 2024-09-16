# 代码随想录 Day20

## 235. [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

[Course Link](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)

和 236 不一样，这题是 BST，因此可以利用特性。若节点的值在 p 和 q 之间，且是第一次遇到这种情况，那么它一定是最小公共祖先。

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val >= min(p.val, q.val) and root.val <= max(p.val, q.val):
            return root
        elif root.val > max(p.val, q.val):
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < min(p.val, q.val):
            return self.lowestCommonAncestor(root.right, p, q)
```

迭代法也非常好写，这里精简的写可以直接把 root.left 或者 right 赋值给 root，不用拿栈来记录

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        stack = [root]
        while stack:
            for _ in range(len(stack)):
                node = stack.pop()
                if node.val <= max(p.val, q.val) and node.val >= min(p.val, q.val):
                    return node
                if node.val > max(p.val, q.val):
                    stack.append(node.left)
                if node.val < min(p.val, q.val):
                    stack.append(node.right)
        return None
```

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while root:
            if root.val < min(p.val, q.val):
                root = root.right
            elif root.val > max(p.val, q.val):
                root = root.left
            else:
                return root
        return None
```
