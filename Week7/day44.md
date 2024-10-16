# 代码随想录 Day44

## 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

[Course Link](https://programmercarl.com/0115.%E4%B8%8D%E5%90%8C%E7%9A%84%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

dp 的每个值代表了当前位置 t 在 s 中有几个。显然有两种情况，一种是 s[i-1] = t[j-1]，另一种是不相等。若相等，那么值就等于左上角加上左边的值，含义是没有这两个当前字符情况下的数量，加上，若不包含当前 s 这个字符情况下的数量。举个例子：s = 'rabb'，t='rab'，检测到 s[3] = t[2]，我们知道 s[2] = t[1] 情况下，'ra' 在 'rab' 中出现了一次，并且我们假设不使用当前 s 的字符，也就是 s = 'rab' 的情况下，t = 'rab' 在 s 中有一次匹配。因此，当前的值就是 1+1=2。还有一种情况是不相等，很显然就是使用 s 没有当前字符的情况下的值。

这样讲很抽象，但是拿一个例子模拟一下就很清晰了。

```
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0]*(len(s)+1) for _ in range(len(t)+1)]
        for i in range(len(s)+1):
            dp[0][i] = 1
        for i in range(1, len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[j][i] = dp[j-1][i-1]+dp[j][i-1]
                else:
                    dp[j][i] = dp[j][i-1]
        return dp[-1][-1]
```

## 583. [Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)

[Course Link](https://programmercarl.com/0583.%E4%B8%A4%E4%B8%AA%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E5%88%A0%E9%99%A4%E6%93%8D%E4%BD%9C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

首先想到的是这题可以先找到最长相同子序列，然后剩下的全删了就是答案。那么结果就是两条string的长度和减去两倍的最长相同子序列的长度。方法和 LC1143是一样的，只是结果小小的计算一下即可。

```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word1)+1) for _ in range(len(word2)+1)]
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[j][i] = dp[j-1][i-1]+1
                else:
                    dp[j][i] = max(dp[j-1][i], dp[j][i-1])
        return len(word1)+len(word2)-2*dp[-1][-1]
```

本题另一种解法就是删除元素。dp[i][j] 的值代表了 word1[:i] 和 word2[:j] 最小删除次数。当然还是有两种可能性，即当前字符相同，和当前字符不相同。相同时，当前值应当和左上角（还没遇到这两个字符时）的值一样。不相同时，有三种操作方式，删 word1的那个字符（即 dp[i-1][j]+1），删 word2 的那个字符（即 dp[i][j-1]+1），两个都删（即 dp[i-1][j-1]+2），我们希望最小化操作次数，因此这三个取最小。
另外初始化是必须的，首先要包含空字符，另外当前 word1[:i] 情况下，word2 删除次数，反之亦然。

```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word1)+1) for _ in range(len(word2)+1)]
        for i in range(1, len(word1)+1):
            dp[0][i] = i
        for i in range(1, len(word2)+1):
            dp[i][0] = i
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[j][i] = dp[j-1][i-1]
                else:
                    dp[j][i] = min(dp[j-1][i]+1, dp[j][i-1]+1, dp[j-1][i-1]+2)
        return dp[-1][-1]
```

## 72. [Edit Distance](https://leetcode.com/problems/edit-distance/)

[Course Link](https://programmercarl.com/0072.%E7%BC%96%E8%BE%91%E8%B7%9D%E7%A6%BB.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

可以做三种操作：删除，添加，替换。那么也就是说两种情况，一种是两个字符相同，不做操作，还有一种是不相同，那么可以做三种操作。dp 的值代表了当前字符情况下最小操作数。对于第一种，当前值就等于左上角的值，相当于不操作。对于第二种：1. 删除，和之前的题一样，dp[j][i] = dp[j][i-1]+1；2. 添加，word1添加一个就相当于word2删除一个，因此，dp[j][i] = dp[j-1][i]+1；3. 替换，相当于 dp[j-1][i-1] 之后再做一个替换操作，因此 dp[j][i] = dp[j-1][i-1]+1。
最后初始化和之前一样，要考虑空字符情况下，最小操作数，也就是每多一个字符就加1。

```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word1)+1) for _ in range(len(word2)+1)]
        for i in range(1, len(word1)+1):
            dp[0][i] = i
        for i in range(1, len(word2)+1):
            dp[i][0] = i
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[j][i] = dp[j-1][i-1]
                else:
                    dp[j][i] = min(dp[j-1][i]+1, dp[j][i-1]+1, dp[j-1][i-1]+1)
        return dp[-1][-1]
```
