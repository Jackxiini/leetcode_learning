# 代码随想录 Day31

## 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

[Course Link](https://programmercarl.com/0056.%E5%90%88%E5%B9%B6%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

挺简单，起始位置排序，如果后一个的起始比之前记录的末尾小，那么就是重叠，然后要记录最大的末尾，再继续。

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x:x[0])
        res = [intervals[0]]
        for i in range(1, len(intervals)):
            if intervals[i][0] <= res[-1][1]:
                res[-1][1] = max(res[-1][1], intervals[i][1])
            else:
                res.append(intervals[i])
        return res
```

## 738. [Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits/)

[Course Link](https://programmercarl.com/0738.%E5%8D%95%E8%B0%83%E9%80%92%E5%A2%9E%E7%9A%84%E6%95%B0%E5%AD%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

倒叙检查，如果当前数比大一位的数小，那么从当前数到位数更小的那些个数都变 9。

```
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        s = list(str(n))
        for i in range(len(s)-1, 0, -1):
            if s[i] < s[i-1]:
                s[i-1] = str(int(s[i-1])-1)
                for j in range(i, len(s)):
                    s[j] = '9'
        return int(''.join(s))
```

## 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

[Course Link](https://programmercarl.com/0968.%E7%9B%91%E6%8E%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

从叶子节点往根节点遍历，并且要分情况讨论，首先一定不可能在叶子节点放摄像头因为浪费。其次根节点放不放要从三点讨论：

1. 左右子节点都被摄像头覆盖了，这时候根节点不放摄像头，返回 0 （没有被覆盖）
2. 左右子节点有一个没有被覆盖，这时候根节点一定要放摄像头，返回 2 （放摄像头）
3. 左右子节点有一个放了摄像头，这时候根节点被覆盖了，返回 1 （被覆盖）

注意的是如果是 None，那么属于被覆盖，因为如果是未覆盖（1）那么叶子节点会被放摄像头（浪费！），如果是放摄像头（2），那么叶子节点会变成被覆盖，导致它的根节点被错误的认为不该放摄像头

二刷再复习

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # 0, not covered; 1, covered; 2, has camera
    def __init__(self):
        self.res = 0

    def traverse(self, node):
        if not node:
            return 1

        left = self.traverse(node.left)
        right = self.traverse(node.right)

        if left == 0 or right == 0:
            self.res += 1
            return 2

        elif left == 1 and right == 1:
            return 0
        
        elif left == 2 or right == 2:
            return 1
        
    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        if self.traverse(root) == 0:
            self.res += 1
        
        return self.res
```
