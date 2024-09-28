# 代码随想录 Day32

开始进入动态规划章节。这里是 [理论基础](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE`)

## 509. [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

[Course Link](https://programmercarl.com/0509.%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

首先是递归，很容易写但是很慢，时间复杂度 O(2^n)

```
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        if n == 1:
            return 1
        return self.fib(n-1) + self.fib(n-2)
```

我们需要使用 DP，要注意 n = 0 的情况

```
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        dp = [0] * (n+1)
        dp[0] = 0
        dp[1] = 1
        for i in range(2, len(dp)):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[-1]
```

## 70. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

[Course Link](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

通过观察可以发现，第一层有一种方法，第二层有两种方法，第三层有三种（第一层的方法再上两层，和，第二层的方法再上一层）。因此第三层有多少种和前两层相关，并且为它们的和，因此我们可以用 DP（dp[i] = dp[i-1]+dp[i-2]）

```
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        dp = [0] * n
        dp[0] = 1
        dp[1] = 2
        for i in range(2, n):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[-1]
```

## 746. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

[Course Link](https://programmercarl.com/0746.%E4%BD%BF%E7%94%A8%E6%9C%80%E5%B0%8F%E8%8A%B1%E8%B4%B9%E7%88%AC%E6%A5%BC%E6%A2%AF.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

简单模拟一下就会发现，当前这一层的最小 cost 是前两层的最小 cost 加上前两层的 cost中最小的那个。因此 dp[i] = min(dp[i-1]+cost[i-1], dp[i-2]+cost[i-2])。要注意的是 dp 需要比 cost 长度加一，因为需要记录到顶点的 cost。

另外，前两层是不需要 cost 的，因此初始化都是 0。

```
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp = [0] * (len(cost)+1)
        dp[0] = 0
        dp[1] = 0
        for i in range(2, len(dp)):
            dp[i] = min(cost[i-1]+dp[i-1], cost[i-2]+dp[i-2]) 
        return dp[-1]
```
