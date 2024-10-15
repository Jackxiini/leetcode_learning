# 代码随想录 Day43

## 1143. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

[Course Link](https://programmercarl.com/1143.%E6%9C%80%E9%95%BF%E5%85%AC%E5%85%B1%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

找到公共的最长子序列，这个序列不需要是连续的。dp 里的值代表了当前 text1 和 text2 的最长公共子序列。初始化第一列第一行用一个 null，方便计算。如果当前字符相同，那么左上角的值 + 1等于当前值，如果不相同，那么取上边和左边的最大值。用一个例子可以很好的解释这个。

```
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0]*(len(text1)+1) for _ in range(len(text2)+1)]
        for i in range(1, len(text1)+1):
            for j in range(1, len(text2)+1):
                if text1[i-1] == text2[j-1]:
                    dp[j][i] = dp[j-1][i-1]+1
                else:
                    dp[j][i] = max(dp[j-1][i], dp[j][i-1])
        return dp[-1][-1]
```

## 1035. [Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/)

[Course Link](https://programmercarl.com/1035.%E4%B8%8D%E7%9B%B8%E4%BA%A4%E7%9A%84%E7%BA%BF.html)

和上一题 LC1143 是一模一样的，不能交叉的匹配，也就是按照顺序找最大公共子序列。

```
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0]*(len(nums1)+1) for _ in range(len(nums2)+1)]
        for i in range(1, len(nums1)+1):
            for j in range(1, len(nums2)+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[j][i] = dp[j-1][i-1] + 1
                else:
                    dp[j][i] = max(dp[j-1][i], dp[j][i-1])
        return dp[-1][-1]
```

## 53. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

[Course Link](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

上次用的贪心做。这次用 dp，dp 的值代表了当前值及之前的最大子序列和。很容易发现我们每次都想要保留当前值和加上之前最大值时的最大值，例如 [-2, 1]，对于 -2，因为是第一个值，我们初始化，使用当前值 -2。但对于 1 这个位置，我们希望使用 1 而不是 [-2, 1]，因为加上之前的值会使现在的值变小。

贪心感觉也差不多，我们保持记录当前最大值，在遍历过程中也要记录当前累加值。一旦累加值小于0了，就立即更新为0，因为我们之前的负值会影响后面的最大值，遇到变负了，subarray的起始就应该重新开始。

```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0]*len(nums)
        dp[0] = nums[0]
        for i in range(1, len(nums)):
            dp[i] = max(nums[i], dp[i-1]+nums[i])
        return max(dp)
```

## 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

[Course Link](https://programmercarl.com/0392.%E5%88%A4%E6%96%AD%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

编辑距离的入门题目，dp 的值代表了当前最大匹配子序列的长度，匹配时，t[j-1] == s[i-1]，dp 值相当于左上角的值加一，不匹配（t[j-1] != s[i-1]）时，dp的值取左边的值，相当于 s 的这个值没有被匹配到（如果被匹配到了，这个值一定是 s[:当前字符] 的长度），最后要检查最后那个值是不是 s 的长度。

```
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        dp = [[0]*(len(t)+1) for _ in range(len(s)+1)]
        for j in range(1, len(t)+1):
            for i in range(1, len(s)+1):
                if t[j-1] == s[i-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = dp[i][j-1]
        return True if dp[-1][-1]==len(s) else False
```

但这种方法对于这题而言，时间复杂度很高，是 O(mn)，用双指针也可以轻松解，时间复杂度为 O(n)。

```
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if not s:
            return True
        j = 0
        for i in range(len(t)):
            if s[j] == t[i]:
                j+=1
            if j == len(s):
                return True
        return False
```
