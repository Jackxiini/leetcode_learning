# 代码随想录 Day40

## 188. [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

[Course Link](https://programmercarl.com/0188.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIV.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

LC 123 的进阶版，这次是可以买卖 k 次。会做 LC123，这题就会做，其实是一样的。状态有 1+2*k 种（LC 123 是该题的特殊情况，k=2，即5种），然后很容易发现，状态是奇数时候是持有（上一个状态减去股价），偶数是不持有（上一个状态加上股价），初始化也是一样的道理。

```
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        situation = k*2+1
        dp = [0] * (situation)
        for i in range(1, situation):
            if i%2 == 1:
                dp[i] = -prices[0]
                
        for price in prices:
            for i in range(1, situation):
                if i%2 == 1:
                    dp[i] = max(dp[i], dp[i-1]-price)
                else:
                    dp[i] = max(dp[i], dp[i-1]+price)
               
        return dp[-1]
```

## 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

[Course Link](https://programmercarl.com/0309.%E6%9C%80%E4%BD%B3%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E6%97%B6%E6%9C%BA%E5%90%AB%E5%86%B7%E5%86%BB%E6%9C%9F.html)

写这题会发现一个问题，之前为什么都可以用一维 dp 做，但是这题不行。举个例子，LC122 可以用一维 dp 做，但是在更新不持有信息的情况下，持有已经被更新了（所以前一个持有信息丢失了），然而我们需要用前一个持有的信息来更新目前不持有的信息啊（也就是 dp[i][1] 得靠 dp[i-1][1] 更新）。那是因为不持有的情况下，仅会在今天股价比昨天高的情况下，我们才会卖出（也就是用到 dp[i-1][0] + price），那么对于今天比昨天股价高的情况，dp[i][0] 又一定是等于 dp[i-1][0] 的，所以这时候信息还是会被正确更新。

那么回到本题，股票卖出后，要冻结一天，才能再买。所以本题可以有三种状态：1，持有（即继续持有和买入），2，卖出，3，卖出后的冻结（即继续不持有）。对于状态 1，就是取昨天持有和昨天冻结今天买入的最大值。对于状态 2，就是求出昨天持有后今天卖出的利润。对于状态 3，就是取昨天卖出和昨天冻结的最大值。这里我们就能发现，由于状态 3 必须由昨天的卖出状态进行比较（因为卖出后要冻一天，所以我们不能拿今天的卖出和昨天的冻结比），所以我们需要获取到 dp[1][i-1] 这个信息，因此用二维 dp。


```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*len(prices) for _ in range(3)]
        dp[0][0] = -prices[0]
        for i in range(1, len(prices)):
            dp[0][i] = max(dp[0][i-1], dp[2][i-1]-prices[i])
            dp[1][i] = dp[0][i-1] + prices[i]
            dp[2][i] = max(dp[1][i-1], dp[2][i-1])
        return max(dp[1][-1],dp[2][-1])
```

但其实我们也可以用一维 dp，只需要在 dp[1] 被更新前存一下老的 dp[1] 信息即可。

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [-prices[0], 0, 0]
        for i in range(1, len(prices)):
            dp[0] = max(dp[0], dp[2]-prices[i])
            pre_1 = dp[1]
            dp[1] = dp[0] + prices[i]
            dp[2] = max(pre_1, dp[2])
        return max(dp[-1],dp[-2])
```

这题挺绕脑子，二刷得再想一想。

## 714. [Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

[Course Link](https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html)

LC122 的带手续费版，区别仅是卖出时候有个手续费，所以卖出那里利润得减去 fee，别的都一样。

```
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        dp = [-prices[0], 0]
        for i in range(1, len(prices)):
            dp[0] = max(dp[0], dp[1]-prices[i])
            dp[1] = max(dp[1], dp[0]+prices[i]-fee)
        return dp[-1]
```
