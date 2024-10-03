# 代码随想录 Day34

## KC 46. [携带研究材料](https://kamacoder.com/problempage.php?pid=1046)

[Course Link](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

经典的01背包问题，用二维数组初始化，横轴是背包重量（从0到给定的重量），纵轴是物品种类，例如背包可载重 6，物品有3种，那么就是一个 7*3 的数组。初始化第一行为只放第一种物品的情况下，每种载重的最大价值，第一列都是0，因为背包载重 0 不能放东西，别的都初始化 0。

DP公式是有两种情况的，一种是放当前物品，那么价值就是当前物品的价值加上剩余载重空间的最佳价值（也就是 value[i] 加上 dp[i-1][j-weight[i]]，因为我们一行行的计算最佳，所以剩余载重空间的最佳价值 dp 数组已经记录了），另一种是不放当前物品，那么价值就是上一行的那个值（dp[i-1][j]），这两个挑最大的作为这个 i, j 的值。

```
n, bagweight = map(int, input().split())

weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [[0]*(bagweight+1) for _ in range(n)]

for i in range(bagweight):
    if dp[0][i]>= weight[0]:
        dp[0][i] = value[0]

for i in range(n):
    for j in range(bagweight+1):
        if j >= weight[i]:
            dp[i][j] = max(dp[i-1][j], value[i]+dp[i-1][j-weight[i]])
	else:
	    dp[i][j] = dp[i-1][j]

print(dp[-1][-1])
```

[Course Link](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html#%E6%80%9D%E8%B7%AF)

但如果画图表就会发现其实不用使用二维数组来初始化 DP，因为每一行 (i) 的那一列 (j) 的那个值只和它上一行 (i-1) 那个值和上一行前几列 (j-n) 某个值有关系，因此可以滚动使用一个一维数组即可。（不理解的话用 Link 里那个例子画个图即可秒懂）

这里有个坑要注意，就是只用一维的话，背包容量不能像二维那样从小到大遍历，得从大到小遍历，因为如果从小到大，那么第 i 个物品就有可能被重复使用。之前用的是上一行 (i-1) 那个值和上一行前几列 (j-n) 某个值，如果是从小到大，现在用的是当前 j 之前的某个值，这个值是有可能已经被更新了的，也就是说已经放了第 i 个物品了，而从大到小的话，更新的值都是当前 j 之后的值，和它之前的无关（画个图就能理解）

```
n, bagweight = map(int, input().split())

weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [0]*(bagweight+1)

for i in range(n):
    for j in range(bagweight, -1, -1):
        if j >= weight[i]:
            dp[j] = max(dp[j], value[i]+dp[j-weight[i]])

print(dp[-1])
```

## 416. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

[Course Link](https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

类似于背包问题，这题里 target 值（也就是数组和的一半）相当于背包容量，每个数是价值和重量。那么只要遍历然后找到某一行的最后一个值等于 target 就是能够切分，跑完没有就是不能。如果和的一半不是整数那么也不能。

二维：

```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums)%2 != 0:
            return False
        target = int(sum(nums)/2)

        dp = [[0]*(target+1) for _ in range(len(nums))]
        for i in range(nums[0], target+1):
            dp[0][i] = nums[0]
        for i in range(1, len(nums)):
            for j in range(1, target+1):
                if nums[i] <= j:
                    dp[i][j] = max(dp[i-1][j], nums[i]+dp[i-1][j-nums[i]])
                else:
                    dp[i][j] = dp[i-1][j]
            if dp[i][-1] == target:
                return True
        return False
```

一维：

```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums)%2 != 0:
            return False
        target = int(sum(nums)/2)

        dp = [0]*(target+1)
        for num in nums:
            for j in range(target, num-1, -1):
                if num <= j:
                    dp[j] = max(num+dp[j-num], dp[j])
                if dp[-1] == target:
                    return True
        return False
```
