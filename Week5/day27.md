
# 代码随想录 Day27

[理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

我愿称之为想到就想到了，想不到就G，没有模板没有固定解题手法。主要思想就是找到局部最优，从而达到全局最优，但是因为不是所有题都适用这个规则，所以得试一试来看是否可用贪心。

## 455. [Assign Cookies](https://leetcode.com/problems/assign-cookies/)

[Course Link](https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

这题还是比较简单和容易想到的，要么就先喂饱胃口大的小孩，要么就先喂饱胃口小的，按照胃口大小排序，饼干大小也要排序。

```
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        res = 0
        i = 0
        j = 0
        while i < len(g) and j < len(s):
            if s[j] >= g[i]:
                res += 1
                i+=1
            j+=1
        return res
```

## 376. [Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/)

[Course Link](https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

有几个坑，首先长度为1的数组，wiggle subseq 的长度也是1。其次，如果之前的 diff 是0，后续出现了上升或者下降，那么也算是一次摆动（例如 [1,1,2]，那么长度是2），结果也要 +1，不能只检查是否是正负数的差异摆动。另外，更新 pre 得在摆动的时候更新，不能在每次迭代里更新，因为我们要记录的是是否摆动。

```
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        max_len = 1
        if len(nums) == 1:
            return max_len
        pre, cur = 0, 0
        for i in range(len(nums)-1):
            cur = nums[i+1]-nums[i]
            if (pre <= 0 and cur > 0) or (pre >= 0 and cur < 0):
                max_len += 1
                pre = cur
        return max_len
```

## 53. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

[Course Link](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html#%E6%80%9D%E8%B7%AF)

代码很简单但是能想到很难，贪心在每次遇到总和小于0了，就会重置总和，开启一段新的计算，但是这整个过程中我们都要记录最大sum

```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = 0
        res = float('-inf')
        for i in nums:
            max_sum += i
            if max_sum > res:
                res = max_sum 
            if max_sum < 0:
                max_sum = 0
        return res
```
