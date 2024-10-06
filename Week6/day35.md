# 代码随想录 Day35

## 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

[Course Link](https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

和 LC 416 是一回事，这次要找的是所有总和一半为 target 的情况下，能找到的最大值，那么差值 (last stone) 就是总和减去两个最大值（也就是分割问题，分割成尽量相等的两组，最小差值是多少）

```
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        max_weight = sum(stones)//2
        dp = [[0]*(max_weight+1) for _ in range(len(stones))]
        for i in range(stones[0], max_weight+1):
            dp[0][i] = stones[0]
        for i in range(1, len(stones)):
            for j in range(1, max_weight+1):
                if j >= stones[i]:
                    dp[i][j] = max(stones[i]+dp[i-1][j-stones[i]], dp[i-1][j])
                else:
                    dp[i][j] = dp[i-1][j]
        return sum(stones)-2*dp[-1][-1]
```

## 494. [Target Sum](https://leetcode.com/problems/target-sum/)

[Course Link](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

感觉很难。首先要明确 dp[i][j] 代表了 [0,i] 这些数凑满 j 值有多少种方法。然后 j 是需要计算出来的，还是切分的问题，已知 sum = left + right，target = left - right，可求得 left = (target + sum)/2。这里就可以发现有些情况是无解的，也就是left不是整数的情况。另外初始化的时候要注意，j = 0的情况下非零数的方法一定是 1（不放 nums[0] ），但是如果 nums[0] 是0那么就是两种（放和不放）。另外如果 j 等于 nums[0]，那么就是一种方法，其余都是0。
dp公式里当前 nums[i] 的方法个数是 不放它的个数 和 放它的个数 的总和。也就是，不放它的个数（dp[i-1][j]）加上 放它的个数（dp[i-1][j-nums[i]]，放它以后剩余值的方法个数）（这里想不明白，看 link 里的绿色框的物品 0，1，2的例子就能明白）。另外如果遇到 nums[i] = 0，那么就是两倍的 dp[i-1][j]，因为放与不放 0 都不会影响总和，但是这是两种独立的方法，所以乘以2。

```
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        if (target+sum(nums))%2!=0 or abs(target) > sum(nums):
            return 0
        new_target = (target+sum(nums))//2
        dp = [[0]*(new_target+1) for _ in range(len(nums))]

        dp[0][0] = 1
        if nums[0] <= new_target:
            dp[0][nums[0]] = 1

        if nums[0] == 0:
            dp[0][0] = 2

        for i in range(1, len(nums)):
            for j in range(new_target+1):
                if j >= nums[i]:
                    dp[i][j] = dp[i-1][j] + dp[i-1][j-nums[i]]
                else:
                    dp[i][j] = dp[i-1][j]

                if nums[i] == 0:
                    dp[i][j] = 2 * dp[i-1][j]
        return dp[-1][-1]
```

## 474. [Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)

[Course Link](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

二维的背包，一维的背包只有一个重量参数，这次是有两个，0的个数和1的个数。所以dp也是二维的（相较于0-1背包的一维dp，用三维的dp也就是加上物品那一维会不好画表，容易搞乱所以还是用二维的）。
其余的就一样了，每次遍历物品（string）的时候就提取有几个0，有几个1，并且注意要逆序遍历背包，从大到小避免重复使用物品。

二刷再复习。

```
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0]*(m+1) for _ in range(n+1)]
        for string in strs:
            zero = string.count('0')
            one = string.count('1')
            for i in range(n, one-1, -1):
                for j in range(m, zero-1, -1):
                    dp[i][j] = max(dp[i][j], dp[i-one][j-zero]+1)
        return dp[-1][-1]
```
