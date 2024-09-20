# 代码随想录 Day24

## 93. [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

[Course Link](https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html#%E6%80%9D%E8%B7%AF)

也是个切割问题，和 LC 131 很像，注意去除不符合要求的数字就行

```
class Solution:
    def backtracking(self, s, res, comb, split):
        if len(comb) == 4 and split == len(s):
            res.append(".".join(comb))
            return

        for i in range(split, len(s)):
            sub = s[split:i+1]
            if int(sub)<0 or int(sub)>255 or (len(sub)!=1 and sub[0]=='0'):
                continue
            if len(comb) > 4:
                break
            comb.append(sub)
            self.backtracking(s, res, comb, i+1)
            comb.pop()

    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []
        self.backtracking(s, res, [], 0)
        return res
```
