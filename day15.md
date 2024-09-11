# 代码随想录 Day15

## 110. [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

[Course Link](https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

用后序遍历，求高度。因为对于中间节点，需要直到它的左右子树情况后才能被更新。
递归还是写不太明白，还得再练练。

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def getheight(node):
            if not node:
                return 0
            left = getheight(node.left)  #left
            right = getheight(node.right)   #right
            if left == -1 or right == -1:
                return -1
            # mid
            elif abs(left-right)>1:
                return -1
            else:
                return 1+max(left, right)

        if getheight(root) == -1:
            return False 
        return True
```

## 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

[Course Link](https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html)

要用回溯算法
还需要再练一练

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        def getpath(node, path, res):
            path.append(str(node.val))
            if not node.left and not node.right:
                res.append('->'.join(path))
            if node.left:
                getpath(node.left, path, res)
                path.pop()
            if node.right:
                getpath(node.right, path, res)
                path.pop()
            
        path = []
        res = []
        getpath(root, path, res)
        return res
```

## 404. [Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)

[Course Link](https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html)

注意要左叶子而不是左节点

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        def traverse(node, if_left, leftsum):
            if node.left:
                leftsum = traverse(node.left, True, leftsum)
            if node.right:
                leftsum = traverse(node.right, False, leftsum)
            if not node.left and not node.right and if_left:
                leftsum += node.val
            return leftsum
        
        leftsum = 0
        leftsum = traverse(root, False, leftsum)
        return leftsum
```
## 222. [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

[Course Link](https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html#%E6%80%9D%E8%B7%AF)

可以用层序遍历或者递归遍历每一个节点来算个数但是这样时间复杂度就是 O(n)了不满足题目要求（小于O(n)）
