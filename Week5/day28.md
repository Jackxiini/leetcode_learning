# 代码随想录 Day28

## 122. [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

[Course Link](https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII.html)

贪心算法，每天涨了就抛。

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        for i in range(1, len(prices)):
            diff = prices[i]-prices[i-1]
            if diff > 0:
                res += diff
        return res
```

## 55. [Jump Game](https://leetcode.com/problems/jump-game/)

[Course Link](https://programmercarl.com/0055.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8F.html#%E6%80%9D%E8%B7%AF)

和 Link 里想法不太一样。我的想法是每次都看看剩余 power 是不是比当前地点的值高，低了就汲取当前的值为新 power。

```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        power = nums[0]
        for i in range(1, len(nums)):
            power -= 1
            if power < 0:
                return False
            if nums[i] > power-1:
                power = nums[i]
        return True
```

## 45. [Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)

[Course Link](https://programmercarl.com/0045.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8FII.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

比前一题难得多，贪心在每次在 cover 范围里找到能 cover 最远的那个节点继续走

```
class Solution:
    def jump(self, nums: List[int]) -> int:
        cur_dest = 0
        far_dest = 0
        res = 0
        for i in range(len(nums)-1):
            far_dest = max(far_dest, nums[i]+i)
            if i == cur_dest:
                res+=1
                cur_dest = far_dest
        return res
```
## 1005. [Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)

[Course Link](https://programmercarl.com/1005.K%E6%AC%A1%E5%8F%96%E5%8F%8D%E5%90%8E%E6%9C%80%E5%A4%A7%E5%8C%96%E7%9A%84%E6%95%B0%E7%BB%84%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

直觉上来说，按照绝对值从大到小排序，然后把负数按大到小翻转变正，如果还多出来 k，看 k 是不是奇数，是奇数就把最小的那个值（一定在数组最右边）变负。

```
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        nums.sort(key=lambda x:abs(x), reverse=True)
        for i in range(len(nums)):
            if k > 0 and nums[i] < 0:
                nums[i] *= -1
                k -= 1
        if k % 2==1:
            nums[-1] *= -1
            
        return sum(nums)
```
