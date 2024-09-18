# 代码随想录 Day21

## 669. [Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

[Course Link](https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)

自己先写了一遍，想法是一探到底（后序遍历）去找有没有需要修剪的节点，再回溯到 root，这样就不会漏掉需要修剪的节点

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if root.left:
            root.left = self.trimBST(root.left, low, high)
        if root.right:
            root.right = self.trimBST(root.right, low, high)
        if root.val < low or root.val > high:
            if not root.left and not root.right:
                return None
            elif not root.left and root.right:
                return root.right
            elif not root.right and root.left:
                return root.left
            elif root.right and root.left:
                cur = root.right
                while cur.left:
                    cur = cur.left
                cur.left = root.left
                return root.right
        
        return root
```

利用一下 BST 的特性，根据大小去找不符合要求的节点去去除，这个代码不太好理解

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return None
        if root.val < low:
            return self.trimBST(root.right, low, high)
        if root.val > high:
            return self.trimBST(root.left, low, high)
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)

        return root
```

## 108. [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

[Course Link](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

找中间数作为中间节点，因为数组已经排序，所以可以直接取左边和右边作为左右节点的数组
递归还得多加练习

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if len(nums)==0:
            return
        mid = len(nums)//2
        node = TreeNode(val=nums[mid])
        num_left = nums[:mid]
        num_right = nums[mid+1:]
        node.left = self.sortedArrayToBST(num_left)
        node.right = self.sortedArrayToBST(num_right)
        return node
```

## 538. [Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

[Course Link](https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html#%E6%80%9D%E8%B7%AF)

用中序遍历，但是用反过来顺序的中序遍历（右中左），可以在 BST 中从大到小遍历，每次都记录叠加的值

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.cur_val = 0

    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return
        self.convertBST(root.right)
        root.val = self.cur_val + root.val
        self.cur_val = root.val
        self.convertBST(root.left)

        return root
```
