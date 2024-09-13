# 代码随想录 Day17

## 654. [Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

[Course Link](https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)

和 LC 105，106 一个解法

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return
        max_val = max(nums)
        root = TreeNode(val=max_val)

        max_val_index = nums.index(max_val)
        nums_left = nums[:max_val_index]
        nums_right = nums[max_val_index+1:]

        root.left = self.constructMaximumBinaryTree(nums_left)
        root.right = self.constructMaximumBinaryTree(nums_right)
        
        return root
```

## 617. [Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

[Course Link](https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html)

自己写了一遍有点复杂

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        root1_left = root2_left = None
        root1_right = root2_right = None
        cur_val = 0
        if root1:
            cur_val += root1.val
            root1_left = root1.left
            root1_right = root1.right
        if root2:
            cur_val += root2.val
            root2_left = root2.left
            root2_right = root2.right
        elif not root1 and not root2:
            return
        root = TreeNode(val=cur_val)
        
        root.left = self.mergeTrees(root1_left, root2_left)
        root.right = self.mergeTrees(root1_right, root2_right)

        return root
```

改进一下，在 root1 基础上加上 root2

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1:
            return root2
        if not root2:
            return root1
        root1.val = root1.val + root2.val
        root1.left = self.mergeTrees(root1.left, root2.left)
        root1.right = self.mergeTrees(root1.right, root2.right)

        return root1
```

## 700. [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

[Course Link](https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html)

不利用 BST 的特性解

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        from collections import deque
        que = deque([root])
        while que:
            for _ in range(len(que)):
                node = que.pop()
                if node.val == val:
                    return node
                if node.left:
                    que.appendleft(node.left)
                if node.right:
                    que.appendleft(node.right)
        return None
```

利用 BST 的特性解

迭代：

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        while root:
            if root.val == val:
                return root
            elif root.val > val:
                root = root.left
            else:
                root = root.right
        return None
```

递归：

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return None
        if root.val > val:
            return self.searchBST(root.left, val)
        elif root.val < val:
            return self.searchBST(root.right, val)
        else:
            return root
```

## 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

[Course Link](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)

有个坑，右边的子树里的 node.val 也得比左边的都大，有可能在右边子树的左枝里它是小于父节点的，但是它也小于左边树里的值。
并且要提防 node.val 相等的情况，这也得 return False

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def inorder(node):
            if not node:
                return
            inorder(node.left)
            res.append(node.val)
            inorder(node.right)
        
        res = []
        inorder(root)
        for i in range(1, len(res)):
            if res[i] <= res[i-1]:
                return False
        return True
```
