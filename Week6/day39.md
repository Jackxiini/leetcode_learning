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

## 122. [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

[Course Link](https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

这次我们可以频繁交易，当天也可以买入卖出。这样我们就可以多次今天买明天（或以后）卖来最大化收益。贪心就是只要股票涨了就卖掉，然后立马再买进（因为可以看到后一天涨不涨，因此后一天涨就当天立马再买进，明天卖掉），总之吃进每一段涨幅。

dp 就是另一个思路了，dp里数值的含义是手里现金是多少（可以是负数）。另外需要定义持有还是不持有，持有的话得比较是当天买进好（昨天不持有的现金减去今天股价）还是维持昨天持有好。不持有的话就要比较是当天卖出好（昨天持有加上今天股价）还是维持昨天不持有好（这两天都不买）

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [-prices[0],0]
        for i in range(1, len(prices)):
            dp[0] = max(dp[0], dp[1]-prices[i])
            dp[1] = max(dp[1], dp[0]+prices[i])
        return dp[1]
```

## 123. [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

[Course Link](https://programmercarl.com/0123.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIII.html)
