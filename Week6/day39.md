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

现在可以买卖两次。这时候状态就会变成5种，不操作，第一次持有，第一次不持有，第二次持有，第二次不持有。这里之所以用持有这个词，是因为状态想表达的是当天手里是否有这支股票。例如第一次持有，可以是昨天就持有着了，今天继续持有，也可以是今天才买入（也就是前一天无操作，注意这里可能有疑惑，为什么不能是前一天卖出，今天再买入？因为这种情况是第二次持有，第一次持有一定是之前没有持有过，也就是之前没有操作）。
那么就好办了，dp 里的值还是指代手里有的现金价值（可以是负数），然后五种状态：一种状态是不操作 (dp[0]，不操作那么都是0)，然后是第一次持有 (dp[1]，肯定是 max(昨天无操作减去今天买入的股价，昨天就持有的价值))，然后第一次不持有 (dp[2]，max(昨天买入今天卖出的收益，昨天就卖出的价值))，然后第二次不持有 (dp[3]，max(昨天不持有的收益减去今天买入的值，昨天就持有的价值))，最后第二次不持有 (dp[4]，max(昨天买入今天卖出的收益，昨天就卖出的价值))。
因为可以一天多次买卖，所以第一天初始化时候，第二次持有的价值和第一次持有是一样的，相当于今天买入 (dp[1]) 卖出 (dp[2]) 再买入 (dp[3]) ，然后还能再卖出 (dp[4])。

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [0]*5
        dp[1] = -prices[0]
        dp[3] = -prices[0]
        for price in prices:
            dp[1] = max(dp[1], dp[0]-price)
            dp[2] = max(dp[2], dp[1]+price)
            dp[3] = max(dp[3], dp[2]-price)
            dp[4] = max(dp[4], dp[3]+price)
        return dp[-1]
```
