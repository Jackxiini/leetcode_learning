
# 代码随想录 Day27

[理论基础](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

我愿称之为想到就想到了，想不到就G，没有模板没有固定解题手法。主要思想就是找到局部最优，从而达到全局最优，但是因为不是所有题都适用这个规则，所以得试一试来看是否可用贪心。

## 455. [Assign Cookies](https://leetcode.com/problems/assign-cookies/)

[Course Link](https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

这题还是比较简单和容易想到的，要么就先喂饱胃口大的小孩，要么就先喂饱胃口小的，按照胃口大小排序，饼干大小也要排序。

```
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        res = 0
        i = 0
        j = 0
        while i < len(g) and j < len(s):
            if s[j] >= g[i]:
                res += 1
                i+=1
            j+=1
        return res
```
