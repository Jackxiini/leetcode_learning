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

## 45. 
