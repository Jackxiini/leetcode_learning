
# 代码随想录 Day18

## 530. [Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

[Course Link](https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html)

最容易想到的是中序遍历然后在获得的递增数组里找到最小差值

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        def inorder(node):
            if not node:
                return
            inorder(node.left)
            order.append(node.val)
            inorder(node.right)
        
        order = []
        inorder(root)
        mindiff = float('inf')
        for i in range(1,len(order)):
            diff = abs(order[i]-order[i-1])
            if diff < mindiff:
                mindiff = diff
        return mindiff
```

也可以在遍历的时候就更新最小差值，这是因为我们是 BST，中序遍历是递增的

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.mindiff = float('inf')
        self.pre = None
        
    def inorder(self, node):
        if not node:
            return
        self.inorder(node.left)
        if self.pre:
            self.mindiff = min(self.mindiff, abs(node.val-self.pre.val))
        self.pre = node
        self.inorder(node.right)

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.inorder(root)
        return self.mindiff
```

## 501. [Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

[Course Link](https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

要注意利用 BST 的特性，并且在往结果集中增加新数的时候，若未来有更大的 mode 出现，应该清空结果集再收集

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.max_mode = 0
        self.cur_mode = 0
        self.res = []
        self.cur_val = None
    
    def inorder(self, node):
        if not node:
            return
        self.inorder(node.left)
        if node.val != self.cur_val:
            self.cur_val = node.val
            self.cur_mode = 1
        else:
            self.cur_mode += 1
        if self.cur_mode > self.max_mode:
            self.max_mode = self.cur_mode
            self.res = [node.val]
        elif self.cur_mode == self.max_mode:
            self.res.append(node.val)
        self.inorder(node.right)
        
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.inorder(root)
        return self.res
```
