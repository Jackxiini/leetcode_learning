# 代码随想录 Day45

## 674. [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

[Course Link](https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

dp 的每个值在这种类别的题里并不代表问题求得值了（也就是有几种回文）。这里值是布尔值，代表了 s[i:j] 这个区间的字符是不是回文。因此本题得用二维 dp，初始化用 False。横轴竖轴都是 s 这个字符串。有两种情况，一种是当前 s[i] != s[j]，那么显然不是回文，保持不变。如果 s[i] = s[j]，那么有三种情况，一种是 i = j，那么本题的定义自己和自己一定是回文，result +=1，当前值 True，另一种，i 和 j 是相邻的，比如 'aa'，这种也是回文，result +=1，当前值 True（这两类可以合并），第三种，i 和 j 中间还夹着字符串，比如 'aba'，'abca' 这种样子的，那么就要看夹着的是不是回文，也就是说 s[i+1:j-1] 是不是回文。

这里遍历顺序有讲究，我们需要慢慢拓展开，而不是像之前的 dp 题是整个遍历。并且我发现其实顺序和倒序都是可以的。举个例子，'abcbc'，我们遍历可以是 'a', 'ab', 'abc' ... 'abcbc'，也可以是 'c', 'cb', 'cbc', ... 'cbcba'。这样遍历保证了我们已经获得了之前字符串是否为回文的解（也就是中间夹着的部分是否是回文）。这样讲比较抽象，可以画一个 dp 表模拟一下马上即可理解。

倒序，那么检查右上那个值是不是 True（dp[i+1][j-1]）

```
class Solution:
    def countSubstrings(self, s: str) -> int:
        dp = [[False]*len(s) for _ in range(len(s))]
        result = 0
        for i in range(len(s)-1, -1, -1):
            for j in range(i, len(s)):
                if s[i] == s[j]:
                    if j - i <= 1:
                        result+=1
                        dp[i][j] = True
                    elif dp[i+1][j-1]:
                        result+=1
                        dp[i][j] = True
        return result
```

正序，那么检查左下那个值是不是 True（dp[i-1][j+1]）

```
class Solution:
    def countSubstrings(self, s: str) -> int:
        dp = [[False]*len(s) for _ in range(len(s))]
        result = 0
        for i in range(len(s)):
            for j in range(i+1):
                if s[i] == s[j]:
                    if i-j <= 1:
                        result+=1
                        dp[i][j] = True
                    elif dp[i-1][j+1]:
                        result+=1
                        dp[i][j] = True
        return result
```

## 516. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

[Course Link](https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

用逆序遍历的逻辑。dp 的值代表了当前位置（s[i:j]）最长回文子序列。逻辑和上题类似，如果相同，那么如果 i 和 j 相邻，那么为 2，i=j，那么为 1，如果不相邻，那么就是之前回文的长度 + 2（因为得加上s[i]和s[j]）。如果不同，这个不太一样，我们还是需要记录最长回文，这时候我们想加入 s[i]和s[j] 不能得到回文，因此我们尝试加上 s[i] 和 加上 s[j]，看看哪个能够获得最长回文，记录那个最大值即可。

以下写法逻辑和上题差不多，而且不需要特别的初始化。

```
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0]*len(s) for _ in range(len(s))]
        for i in range(len(s)-1, -1, -1):
            for j in range(i, len(s)):
                if s[i] == s[j]:
                    if j-i<=1:
                        dp[j][i] = 1+j-i
                    else:
                        dp[j][i] = dp[j-1][i+1]+2
                else:
                    dp[j][i] = max(dp[j-1][i], dp[j][i+1])
        return dp[-1][0]
```

也可以将 s[i] == s[j] 这个逻辑里的内容合并，但是这就需要初始化，将对角线的值都设成1。画个表就能理解。

```
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0]*len(s) for _ in range(len(s))]
        for i in range(len(s)):
            dp[i][i] = 1
        for i in range(len(s)-1, -1, -1):
            for j in range(i+1, len(s)):
                if s[i] == s[j]:
                    dp[j][i] = dp[j-1][i+1]+2
                else:
                    dp[j][i] = max(dp[j-1][i], dp[j][i+1])
        return dp[-1][0]
```
