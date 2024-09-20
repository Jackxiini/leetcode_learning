# 代码随想录 Day24

## 93. [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

[Course Link](https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html#%E6%80%9D%E8%B7%AF)

也是个切割问题，和 LC 131 很像，注意去除不符合要求的数字就行

```
class Solution:
    def backtracking(self, s, res, comb, split):
        if len(comb) == 4 and split == len(s):
            res.append(".".join(comb))
            return

        for i in range(split, len(s)):
            sub = s[split:i+1]
            if int(sub)<0 or int(sub)>255 or (len(sub)!=1 and sub[0]=='0'):
                continue
            if len(comb) > 4:
                break
            comb.append(sub)
            self.backtracking(s, res, comb, i+1)
            comb.pop()

    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []
        self.backtracking(s, res, [], 0)
        return res
```

## 78. [Subsets](https://leetcode.com/problems/subsets/)

[Course Link](https://programmercarl.com/0078.%E5%AD%90%E9%9B%86.html)

子集问题，就是画树状图然后遍历整个树，获取整个树里的每个节点结果

```
class Solution:
    def backtracking(self, nums, res, comb):
        res.append(comb[:])

        for i in range(len(nums)):
            comb.append(nums[i])
            self.backtracking(nums[i+1:], res, comb)
            comb.pop()

    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, res, [])
        return res
```

## 90. [Subsets II](https://leetcode.com/problems/subsets-ii/)

[Course Link](https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html)

LC 40 和 78 的结合，如果用 78 的方法，那么结果会有重复，因为数组里有重复数字，所以需要先排序，在遇到前一个数和当前数一样的情况下直接跳过

```
class Solution:
    def backtracking(self, nums, res, comb):
        res.append(comb[:])
        for i in range(len(nums)):
            if i > 0 and nums[i-1] == nums[i]:
                continue
            comb.append(nums[i])
            self.backtracking(nums[i+1:], res, comb)
            comb.pop()

    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()
        self.backtracking(nums, res, [])
        return res
```
