# 代码随想录 Day25

## 491. [Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)

[Course Link](https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html)

根据题意不能给 input 先排序，然后又得去重，所以得使用 used 数组记录当前层用过的数。另外，输出的子数组都得是正序，所以得在递归层面检查是否比上一层的数大（或等于）

```
class Solution:
    def backtracking(self, nums, res, comb, start, pre ,used):
        if len(comb) > 1:
            res.append(comb[:])

        for i in range(start, len(nums)):
            if pre > nums[i] or nums[i] in used:
                continue
            comb.append(nums[i])
            used.add(nums[i])
            self.backtracking(nums, res, comb, i+1, nums[i], set())
            comb.pop()

    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, res, [], 0, float('-inf'), set())
        return res
```

## 46. [Permutations](https://leetcode.com/problems/permutations/)

[Course Link](https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

求排列，并且 input 里没有重复元素。只需要跳过已经用过的数，别的都一样。

```
class Solution:
    def backtracking(self, nums, res, comb):
        if len(comb) == len(nums):
            res.append(comb[:])
            return
        
        for i in range(len(nums)):
            if nums[i] in comb:
                continue
            comb.append(nums[i])
            self.backtracking(nums, res, comb)
            comb.pop()

    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, res, [])
        return res
```

## 47. [Permutations II](https://leetcode.com/problems/permutations-ii/)

[Course Link](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)

因为有重复数字了所以不能用 LC 46 里面那种检查是否出现过的写法了。

链接里还有另一种写法，这里贴上自己的写法，每次传入 nums 的数组都是去除这一次使用过的数的数组，但是要提前记录一下原始 nums 的长度，来保证终止条件里的长度是原始长度（用 len(nums) 是错误的，因为长度会变）

```
class Solution:
    def backtracking(self, nums, res, comb, used, leng):
        if len(comb) == leng:
            res.append(comb[:])
            return 
        
        for i in range(len(nums)):
            if nums[i] in used:
                continue
            comb.append(nums[i])
            used.add(nums[i])
            self.backtracking(nums[:i]+nums[i+1:], res, comb, set(), leng)
            comb.pop()

    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res = []
        leng = len(nums)
        self.backtracking(nums, res, [], set(), leng)
        return res
```
