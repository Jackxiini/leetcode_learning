# 代码随想录 Day33

## 62. [Unique Paths](https://leetcode.com/problems/unique-paths/)

[Course Link](https://programmercarl.com/0062.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

机器人走格子，很经典的题。DP 的方法很容易想到，就是当前这个位置的可能性是它上方和左方的可能性之和。

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1]*n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if i > 0 and j > 0:
                    dp[i][j] = dp[i-1][j] + dp[i][j-1]
                if i == 0:
                    dp[i][j] = dp[i][j-1]
                if j == 0:
                    dp[i][j] = dp[i-1][j]
        return dp[-1][-1]
```

还有种数论做法，很难想到。已知一定要走 m+n-2 步，那么就能发现其中 m-1 步是往下走的，并且这一定要走这些步，不管怎么走。那么我们就可以用组合 C(m+n-2, m-1) 算出结果

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        return comb(m+n-2, m-1)
```

## 63. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

[Course Link](https://programmercarl.com/0063.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84II.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

还是挺好想到的，首先就能想到可以把障碍物当作边缘考虑。我觉得坑的地方在于，要考虑 edge cases，例如起始点就是一个障碍物，还有终点是一个障碍物，这都是有可能的。

```
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0] == 1 or obstacleGrid[-1][-1] == 1:
            return 0
        dp = obstacleGrid.copy()
        dp[0][0] = -1
        for i in range(len(dp)):
            for j in range(len(dp[0])):
                if dp[i][j] == 1:
                    continue
                if i > 0 and j > 0 and dp[i-1][j]!=1 and dp[i][j-1]!=1:
                    dp[i][j] = dp[i-1][j] + dp[i][j-1]
                elif (i == 0 or dp[i-1][j] == 1) and (j > 0 and dp[i][j-1]!=1):
                    dp[i][j] = dp[i][j-1]
                elif (j == 0 or dp[i][j-1] == 1) and (i > 0 and dp[i-1][j]!=1):
                    dp[i][j] = dp[i-1][j]
        return -dp[-1][-1]
```

或者先初始化第一行和第一列，遇到障碍物，后续的就都是0（因为只往右或者往下是到不了那的）

```
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0] == 1 or obstacleGrid[-1][-1] == 1:
            return 0
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0]*n for _ in range(m)]
        for i in range(n):
            if obstacleGrid[0][i] == 0:
                dp[0][i] = 1
            else:
                break
        for i in range(m):
            if obstacleGrid[i][0] == 0:
                dp[i][0] = 1
            else:
                break
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 1:
                    continue
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[-1][-1]
```

## 343. [Integer Break](https://leetcode.com/problems/integer-break/)

[Course Link](https://programmercarl.com/0343.%E6%95%B4%E6%95%B0%E6%8B%86%E5%88%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

首先试几个数看，会发现尽可能拆成好几个 3 的形式，就是最终答案。如果拆 3，发现剩下的值为 4，那么直接使用 4。因为要拆成至少两个数，所以 2 和 3 这两个数的答案就是 1 (1*1) 和 2 (1\*2)

```
class Solution:
    def integerBreak(self, n: int) -> int:
        if n == 2:
            return 1
        if n == 3:
            return 2
        elif n % 3 != 1:
            return (3**(n//3)) * max((n%3),1)
        else:
            return 3**(n//3-1) * (n%3+3) 
```
