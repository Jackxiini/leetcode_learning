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

那么怎么用 DP 解这题呢？当前数的最大值一定是它切分数的最大值和余数乘积的最大值。例如，9 的最大值是 6 的最大值乘以余数 3（或者 1  乘以本身或者 6 乘以 3，这三个中的最大值），因此我们需要遍历来获取比 9 小的数的最大值，来找到 9 的最大值。

```
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[2] = 1
        for i in range(3, n+1):
            for j in range(1, i//2+1):
                dp[i] = max(dp[i-j]*j, j*(i-j), dp[i])
        return dp[n]
```

## 96. [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

[Course Link](https://programmercarl.com/0096.%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF)

感觉难得很，不容易想明白。

先从前三个数推导，n=1只有一种可能，根为1; n=2，有两种可能，根为 1 和 2; n=3，有五种可能，根为 1 到 3。从这三个例子可以发现，树的 root 是 1到 n，因为是 BST，因此每种数字作为根的可能性是左边数量的可能性乘以右边数量的可能性（什么意思呢？举个例子，n=3，根这时候取 1，那么左边一定没有子树（也就是无数可用），右边可以用两个数，这两个数能有多少种树的可能性？就是 n=2 的情况的答案。以此类推，根是 2，左边有一个数，右边也有一个数，两边都是 n=1 的可能性，根是 3，左边两个数，右边没有数，左边是 n=2 的可能性，再把这三个根的可能性相加就是 n=3 的可能性）

“右边可以用两个数，这两个数能有多少种树的可能性？就是 n=2 的情况的答案。” 这里可能会想不明白，为什么右边可用两个数，就是 n=2 的答案，这俩数不一定是 1 和 2 啊。其实不用管数字是多少，只要知道有两个比根大的数就行，打个比方 [1,2] 这俩数能组成的树的可能性是和 [4,5] 这俩数能组成的树的可能性是一样的。

```
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[0] = 1
        for i in range(1, n+1):
            for j in range(1, i+1):
                dp[i] += dp[i-j] * dp[j-1]
        return dp[-1]
```
