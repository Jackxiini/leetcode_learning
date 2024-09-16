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

## 701. [Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

[Course Link](https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

这题如果知道只需要在遇到空节点的时候插入节点就会很简单，遍历 BST ，遇到空节点，看父节点和目标数比较谁大谁小来决定插入到左还是右

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val=val)
        if root.val < val:
            root.right = self.insertIntoBST(root.right, val)
        if root.val > val:
            root.left = self.insertIntoBST(root.left, val)
        return root
```

这个要注意如果 root 也是空的情况

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.parent = None
    
    def traverse(self, node, val):
        if not node:
            node = TreeNode(val)
            if self.parent.val < val:
                self.parent.right = node
            else:
                self.parent.left = node
        
        self.parent = node
        if node.val < val:
            self.traverse(node.right, val)
        if node.val > val:
            self.traverse(node.left, val)

    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            root = TreeNode(val)
        self.traverse(root, val)
        return root
```

## 450. [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

[Course Link](https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

有五种可能性：

1. 找不到对应的值，遍历到空节点，返回 root
2. 找到节点，左右都是空，直接删除
3. 找到节点，左空，右不空，右孩子补位上去
4. 找到节点，左不空，右空，左孩子补位上去
5. 找到节点，左右都不空，把左孩子补到右孩子的最左孩子的左孩子

自己写了一遍相当的复杂，感觉可以精简

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.parent = None
        self.tmp = None

    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return root
        if root.val < key:
            self.parent = root
            self.deleteNode(root.right, key)
        elif root.val > key:
            self.parent = root
            self.deleteNode(root.left, key)
        else:
            if self.parent:
                if root.right:
                    self.tmp = root.right
                    while self.tmp.left:
                        self.tmp = self.tmp.left
                    self.tmp.left = root.left
                    if self.parent.val < key:
                        self.parent.right = root.right
                    elif self.parent.val > key:
                        self.parent.left = root.right
                else:
                    if self.parent.val < key:
                        self.parent.right = root.left
                    elif self.parent.val > key:
                        self.parent.left = root.left
            else:
                if root.right:
                    self.tmp = root.right
                    while self.tmp.left:
                        self.tmp = self.tmp.left
                    self.tmp.left = root.left
                    return root.right
                else:
                    return root.left
            
        return root
```

根据可能性精简了一下递归法

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return root
        if root.val < key:
            root.right = self.deleteNode(root.right, key)
        if root.val > key:
            root.left = self.deleteNode(root.left, key)
        if root.val == key:
            if not root.right and not root.left:
                return None
            if not root.right and root.left:
                return root.left
            if not root.left and root.right:
                return root.right
            else:
                tmp = root.right
                while tmp.left:
                    tmp = tmp.left
                tmp.left = root.left
                return root.right
        return root
```
