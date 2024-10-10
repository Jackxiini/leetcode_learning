# 代码随想录 Day39

## 121. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

[Course Link](https://programmercarl.com/0121.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

之前做过贪心法，现在用 dp 做。每天的策略就是买或者不买，dp里记录买或不买的情况下，当前手里的资金是多少，买了就是负price，卖了就是当前 price 减去买入价格。那么我们就需要一个长度为2的 dp 数组。第一个位置记录买入的最佳价格，第二个位置记录卖出后的最佳价值。

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [-prices[0],0]
        for price in prices:
            if price < -dp[0]:
                dp[0] = -price
            dp[1] = max(dp[1], price+dp[0])
        return dp[1]
```
