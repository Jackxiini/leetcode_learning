# 代码随想录 Day36

### 完全背包问题

[Course Link](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

和 01背包 的区别就是，完全背包的物品可以使用无数次，而 01 背包只能用一次。在实现层面上来说，在01背包中一个坑就是使用一维 dp 的时候我们需要逆序遍历背包的载重，为的就是每个物品只用一次，不会重复使用。而完全背包我们就是要重复使用物品，所以我们不用逆序遍历，而是顺序遍历。

#### KC 52. [携带研究材料](https://kamacoder.com/problempage.php?pid=1052)

```
n,v = map(int,input().split())
weight=[]
values=[]
for i in range(n):
    wi,vi = map(int,input().split())
    weight.append(wi)
    values.append(vi)

dp = [0]*(v+1)
for i in range(n):
    for j in range(v+1):
        if j >= weight[i]:
            dp[j] = max(dp[j-weight[i]]+values[i], dp[j])
print(dp[-1])
```

## 518. [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

[Course Link](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

非常典型的完全背包问题，dp里面数值的含义是多少种方法，这里是用一维 dp 会方便点，因为是完全背包所以顺序遍历，要用到重复。初始化的时候第一个值得是 1，也就是找 0 元有一种方法，这里用 1 还是 0 试一个例子就可以确定。
另外，这里我们只要求出个数目就行了，至于先找什么后找什么，不重要，先给1后给5，和先给5后给1属于一种方法，因此遍历先物品后目标数

```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0]*(amount+1)
        dp[0] = 1
        for i in range(len(coins)):
            for j in range(amount+1):
                if j >= coins[i]:
                    dp[j] = dp[j]+dp[j-coins[i]]
        return dp[-1]
```

## 377. [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

[Course Link](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

感觉用背包问题想这题是怎么也想不明白为什么 排列 是先遍历目标数再遍历物品的（例如每个 dp[j] 代表的真实含义是什么，是方法数，但是是哪些方法），虽然这样去做一定是对的。追求ACC套公式的去做，如果顺序没关系就先物品后目标，如果顺序有关系就先目标后物品是可以的。

但要想明白还是得把本题看作是爬楼梯，一共有 target 层楼梯，可用的步数为 nums，有几种爬法。

参考[题解1](https://leetcode.cn/problems/combination-sum-iv/solutions/1/zhe-dao-ti-bu-jiu-shi-pa-lou-ti-yao-by-j-h5nn/)，[题解2](https://leetcode.cn/problems/combination-sum-iv/solutions/2663854/zu-he-zong-he-ivshu-xue-tui-dao-xian-gou-ap8y/)

```
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0]*(target+1)
        dp[0] = 1

        for j in range(target+1):
            for i in range(len(nums)):
                if j >= nums[i]:
                    dp[j] += dp[j-nums[i]]
        return dp[-1]
```

## 70. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

[Course Link](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

那么爬楼梯也可以看作完全背包问题，和上一题是一模一样的，只是 nums 只能是 1 和 2。

```
class Solution:
    def climbStairs(self, n: int) -> int:
        step = [1,2]
        dp = [0]*(n+1)
        dp[0] = 1
        for j in range(n+1):
            for i in range(2):
                if j >= step[i]:
                    dp[j] += dp[j-step[i]]
        return dp[-1]
```
