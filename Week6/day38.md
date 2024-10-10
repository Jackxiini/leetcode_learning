# 代码随想录 Day38

## 198. [House Robber](https://leetcode.com/problems/house-robber/)

[Course Link](https://programmercarl.com/0198.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

一开始想的太复杂了，想用一个二维dp记录每种 sum 的最大的可能性。时间复杂度很高，但是还是能过。

```
class Solution:
    def rob(self, nums: List[int]) -> int:
        total = sum(nums)
        dp = [[0]*(total+1) for _ in range(len(nums)+1)]
        for i in range(nums[0], total+1):
            dp[1][i] = nums[0]
        for i in range(1, len(nums)+1):
            for j in range(1, total+1):
                if j >= nums[i-1]:
                    dp[i][j] = max(dp[i-1][j], nums[i-1]+dp[i-2][j-nums[i-1]])
        return dp[-1][-1]
```

其实只要用一个一维 dp 挨个试每个位置的最大可能值就行了。初始化要注意在 dp 第一位设 0，然后再把第二位的最大值取 nums 的第一个数，意思就是起步时候没有赚钱，只跑到第一间房，最大是第一间的价值。

```
class Solution:
    def rob(self, nums: List[int]) -> int:
        dp = [0]*(len(nums)+1)
        dp[1] = nums[0]
        for i in range(2, len(nums)+1):
            dp[i] = max(dp[i-1], dp[i-2]+nums[i-1])
        return dp[-1]
```

## 213. [House Robber II](https://leetcode.com/problems/house-robber-ii/)

[Course Link](https://programmercarl.com/0213.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8DII.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

和上一题的区别就是这次房子是环状的，首间和末间是联通的，所以这两个不能同时抢，那么就会发现有三种情况：1. 抢首间和别的，但是不能抢末尾。2. 抢末尾，但是不能抢首个。3. 首末都不能抢。这里用一个例子就能发现第三种是包含于 1和 2 的，所以可以剔除。那么代码就是输出 1 和 2 的两种结果，取个最大。

```
class Solution:
    def rob(self, nums: List[int]) -> int:
        def dps(nums):
            dp = [0]*(len(nums)+1)
            dp[1] = nums[0]
            for i in range(2, len(nums)+1):
                dp[i] = max(dp[i-1], nums[i-1]+dp[i-2])
            return dp[-1]
            
        if len(nums) == 1:
            return nums[0]
        result1 = dps(nums[:-1])
        result2 = dps(nums[1:])
        return result1 if result1>result2 else result2
```

## 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

[Course Link](https://programmercarl.com/0337.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8DIII.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

树形的 dp 问题。相邻节点不能同时抢。首先确定树的遍历方式，这里需要我们要把两边遍历完最后查询 root 的取值（抢或不抢，最大值多少），所以得用后序遍历（左右中）。然后每个 node 有两种状态，抢或者不抢，所以 dp 只需要两个值，(0, 0)，第一个代表不抢时当前最大值，第二个时抢的时候当前最大。初始化是当 node 是 null 的时候，那么返回 (0, 0)，从底往上遍历，每个节点都要返回 dp 到上层。

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def traverse(self, root):
        if not root:
            return (0, 0)
        left = self.traverse(root.left)
        right = self.traverse(root.right)
        # not using cur
        val0 = max(left) + max(right)
        val1 = root.val + left[0] + right[0]
        return (val0, val1)

    def rob(self, root: Optional[TreeNode]) -> int:
        res = self.traverse(root)
        return max(res)
```
