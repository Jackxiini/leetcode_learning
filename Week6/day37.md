# 代码随想录 Day37

## 322. [Coin Change](https://leetcode.com/problems/coin-change/)

[Course Link](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

这次是求最小使用数，所以 dp 方程里要求的是当前最小值和使用当前数后的最小值的最小值。然后初始化这里需要思考一下，如果都使用 0 那么什么结果都会是 0 了，因为任何地方的最小值都不会小于 0，那么就要思考使用正无穷，但是第一个数还得是 0，试一个例子就能发现为什么不能是正无穷（道理上也是 0，要凑 0 元，那么最小需要 0 个硬币）。

```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')]*(amount+1)
        dp[0] = 0
        for i in range(1, amount+1):
            for j in range(len(coins)):
                if i >= coins[j]:
                        dp[i] = min(dp[i-coins[j]]+1, dp[i])
        return dp[-1] if dp[-1]!=float('inf') else -1
```

## 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

[Course Link](https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html)

和上一题 322 是一回事，也是求最小使用数量。差别是需要弄清楚 ”物品“ 是怎么来的，其实就是给目标数 n 开个根号往下取整。

```
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')]*(n+1)
        dp[0] = 0
        nums = int(n**0.5)
        for i in range(1, n+1):
            for j in range(1, nums+1):
                if i >= j**2:
                    dp[i] = min(dp[i], dp[i-j**2]+1)
        return dp[-1] if dp[-1]!=float('inf') else -1
```

## 139. [Word Break](https://leetcode.com/problems/word-break/)

[Course Link](https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

把字符串 s 当作背包，把 wordDict 当作物品。需要初始化一个空字符，空字符的值用 1 （1是True，0是False）。然后就显而易见了。这里要注意的是下标很容易搞错（例如 s[i-len(j):i]，但试一个例子就可以搞清楚）

```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [0]*(len(s)+1)
        dp[0] = 1
        for i in range(1, len(s)+1):
            for j in wordDict:
                if len(j) <= i:
                    if s[i-len(j):i] == j:
                        dp[i] = (1 and dp[i-len(j)]) or dp[i]
        return True if dp[-1]==1 else False
```
