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
