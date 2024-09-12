# 代码随想录 Day16

## 513. [Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

[Course Link](https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html)

层序遍历很简单

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        from collections import deque
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
                
        return level[0]
```

递归困难一下，主要注意优先向左前进，并且要保存当前最深的深度

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        def traversal(node, depth):
            if depth > res[1]:
                res[0] = node.val
                res[1] = depth
            if node.left:
                traversal(node.left, depth+1)
            if node.right:
                traversal(node.right, depth+1)

        res = [root.val, 1] # node_val, depth
        traversal(root, res[1])
        return res[0]
```

## 112. [Path Sum](https://leetcode.com/problems/path-sum/)

[Course Link](https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html#%E6%80%9D%E8%B7%AF)

要注意数字可能是负数，因此不能说当前值大于 targetSum 了就可以返回 False

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        def traversal(node, cur_sum):
            if not node:
                return False
            cur_sum += node.val
            if not node.left and not node.right:
                return cur_sum == targetSum
            return traversal(node.left, cur_sum) or traversal(node.right, cur_sum)
        
        return traversal(root, 0)
```

## 113. [Path Sum II](https://leetcode.com/problems/path-sum-ii/)

[Course Link](https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html#%E6%80%9D%E8%B7%AF)

其中 path 一定要写成 path[:]，即复制一份 path 传给 function（当前节点接下来要做的动作），不然会在原 path 上做修改导致错误

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        def traversal(node, cur_sum, path):
            if not node:
                return
            cur_sum += node.val
            path.append(node.val)
            if not node.left and not node.right:
                if cur_sum == targetSum:
                    res.append(path)
            if node.left:
                traversal(node.left, cur_sum, path[:])
            if node.right:
                traversal(node.right, cur_sum, path[:])

        res = []
        traversal(root, 0, [])
        return res
```

## 106. [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

[Course Link](https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

挺难，关键是找到分割点（root），用 postorder找到 root，用 inorder 和 root 找到左边和右边
代码时间复杂度是 O(n)，但是好像比 LC 别的同样时间复杂度的方法慢了一截

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not postorder:
            return 
        root_val = postorder[-1]
        root = TreeNode(val=root_val)

        split_point = inorder.index(root_val)
        size_left = split_point
        size_right = len(inorder)-split_point-1

        inorder_left = inorder[:size_left]
        inorder_right = inorder[-size_right:]

        postorder_left = postorder[:size_left]        
        postorder_right = postorder[size_left:size_left+size_right]
        
        root.left = self.buildTree(inorder_left, postorder_left)
        root.right = self.buildTree(inorder_right, postorder_right)

        return root
```

## 105. [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

同 106，值得注意的是用 Preorder 和 Postorder 是无法构建 unique 的树的，因为无法分辨左右。

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return
        root_val = preorder[0]
        root = TreeNode(val=root_val)

        split_point = inorder.index(root_val)
        size_left = split_point

        inorderleft = inorder[:size_left]
        inorderright = inorder[1+size_left:]

        preorderleft = preorder[1:size_left+1]
        preorderright = preorder[1+size_left:]

        root.left = self.buildTree(preorderleft, inorderleft)
        root.right = self.buildTree(preorderright, inorderright)

        return root
```
