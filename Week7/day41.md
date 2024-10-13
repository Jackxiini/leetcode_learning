# 代码随想录 Day41

## 300. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

[Course Link](https://programmercarl.com/0300.%E6%9C%80%E9%95%BF%E4%B8%8A%E5%8D%87%E5%AD%90%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

dp 的数值意味着当前数为尾的时候，最长递增子序列多长。遍历数组的每个数，当前数的值相当于它之前的比它小的数的值加一和它当前值的最大值。以此类推。

```
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1]*len(nums)
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j]+1)
        return max(dp)
```

但这个方法复杂度要 N^2，贪心加二分法可以降到 N*logN。这种方法要模拟一下以防 left，right 还有什么时候更新新值出错。二刷再仔细研究一下。

```
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        tails = [nums[0]]
        for num in nums[1:]:
            if num > tails[-1]:
                tails.append(num)
            else:
                left, right = 0, len(tails)-1
                while left < right:
                    mid = (left+right)//2
                    if tails[mid] < num:
                        left = mid + 1
                    else:
                        right = mid
                tails[left] = num
        return len(tails)
```

## 674. [Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

[Course Link](https://programmercarl.com/0674.%E6%9C%80%E9%95%BF%E8%BF%9E%E7%BB%AD%E9%80%92%E5%A2%9E%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

非常简单，而且直觉就能发现，只要当前比之前大，就之前的值加一，不然就从头算起（也就是值为 1）。

```
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        dp = [1]*len(nums)
        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                dp[i] = dp[i-1] + 1
        return max(dp)
```

贪心也是一样的思路，只不过不用 dp 数组，因此空间复杂度降为 O(1).

```
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        count = 1
        result = 1
        for i in range(1, len(nums)):
            if nums[i] > nums[i-1]:
                count+=1
            else:
                count=1
            result = max(result, count)
        return result
```

## 718. [Maximum Length of Repeated Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

[Course Link](https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html)

用二维 dp 代码更清晰一些。两个维度分别是两个数组，第一列第一行用 0 初始化，方便计算，从下标 1 开始才是两个数组的第一个数。遍历过程中两数相等，那么就用左上角那个数加一作为当前值，否则不动，还是0。用 result 记录当前最大值，不然就得算出完整 dp 后找到其中最大值。

```
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0]*(len(nums1)+1) for _ in range(len(nums2)+1)]
        result = 0

        for j in range(1, len(nums1)+1):
            for i in range(1, len(nums2)+1):
                if nums1[j-1] == nums2[i-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                    result = max(result, dp[i][j])
        return result
```

但这个方法不是最优，二分法可以更快，二刷再研究。
