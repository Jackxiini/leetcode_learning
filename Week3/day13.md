# 代码随想录 Day13

## 二叉树的三种遍历

[Course Link](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

## 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)


```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(node):
            if not node:
                return
            res.append(node.val)
            dfs(node.left)
            dfs(node.right)
        res = []
        dfs(root)
        return res
```

## 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

```
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(node):
            if not node:
                return 
            dfs(node.left)
            dfs(node.right)
            res.append(node.val)
        
        res = []
        dfs(root)
        return res
```

## 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def dfs(node):
            if not node:
                return
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        res = []
        dfs(root)
        return res
```
## 层序遍历
[Course Link](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html)

一种写法可以解很多题，关键是用队列记录每一层的node
## 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        from collections import deque
        if not root:
            return 
        que = deque([root])
        res = []
        while que:
            tmp = []
            size = len(que)
            for _ in range(size):
                node = que.pop()
                tmp.append(node.val)
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
            res.append(tmp)
        return res
```
## 107. [Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)
实际上就是上一题取个反
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrderBottom(self, root: Optional[TreeNode]) -> List[List[int]]:
        from collections import deque
        if not root:
            return
        res = []
        que = deque([root])
        while que:
            level = []
            for _ in range(len(que)):
                node = que.pop()
                level.append(node.val)
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
            res.append(level)
        return res[::-1]
```
## 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)
实际上就是取每一层的最后一个值
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        from collections import deque
        if not root:
            return 
        que = deque([root])
        res = []
        while que:
            for _ in range(len(que)):
                node = que.pop()
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
            res.append(node.val)
        return res
```
## 637. [Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/)
取每一层的平均
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        from collections import deque
        que = deque([root])
        res = []
        while que:
            level = 0
            size = len(que)
            for _ in range(size):
                node = que.pop()
                level+=node.val
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
            res.append(round(level/size, 5))
        return res
```
## 429.[N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/description/)
小改一下层序遍历里的left，right，遍历children即可
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children
"""

class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        from collections import deque
        if not root:
            return
        que = deque([root])
        res = []
        while que:
            level = []
            for _ in range(len(que)):
                node = que.pop()
                level.append(node.val)
                for i in node.children:
                    que.appendleft(i)
            res.append(level)
        return res
```
## 515.[Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/description/)
存每一层的最大值
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def largestValues(self, root: Optional[TreeNode]) -> List[int]:
        from collections import deque
        if not root:
            return 
        que = deque([root])
        res = []
        while que:
            max_val = float('-inf')
            for _ in range(len(que)):
                node = que.pop()
                if node.val > max_val:
                    max_val = node.val
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
            res.append(max_val)
        return res
```
## 116.[Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/)
需要注意存前一个node，才能指向当前node
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        from collections import deque
        if not root:
            return
        que = deque([root])
        while que:
            pre = None
            size = len(que)
            for i in range(size):
                node = que.pop()
                if i!=0:
                    pre.next = node
                pre = node
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
        return root
```
## 117.[Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/)
虽然是不一样的树，但是和上一题是一模一样的解法
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Node') -> 'Node':
        from collections import deque
        if not root:
            return
        que = deque([root])
        while que:
            pre = None
            size = len(que)
            for _ in range(size):
                node = que.pop()
                if pre:
                    pre.next = node
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
                pre = node
        return root
```
## 104.[Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
很简单，只需要记录循环多少次即可
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        from collections import deque
        depth = 0
        if not root:
            return depth
        que = deque([root])
        while que:
            size = len(que)
            for _ in range(size):
                node = que.pop()
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
            depth+=1
        return depth
```
## 111.[Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
左右孩都是空的时候说明到达最小深度了
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        from collections import deque
        depth = 0
        if not root:
            return depth
        que = deque([root])
        while que:
            depth += 1
            size = len(que)
            for _ in range(size):
                node = que.pop()
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
                if not (node.right or node.left):
                    return depth
        return depth
```
