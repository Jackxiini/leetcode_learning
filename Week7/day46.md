# 代码随想录 Day46

## 739. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

[Course Link](https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html)

暴力解法就是找每个元素往后数多少个，才能找到比它高的值，但这是过不了的，时间复杂度太高。

用单调栈可以降低时间复杂度至 O(n)。需要创建一个单调栈（用来储存 index），还有个 res 数组。遍历数组，如果当前数比栈头的数小，那么 push 进去，不然就把栈头数 pop 出来，然后拿这个 pop 出来的 index 在 res 里更新。由于我们使用单调递增栈，因此栈内永远保持从头算起的递增，这样可以一次遍历，找到所有天数的结果。

```
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stack = [0]
        res = [0]*len(temperatures)
        for i in range(1, len(temperatures)):
            while stack and temperatures[i] > temperatures[stack[-1]]:
                ind = stack.pop()
                res[ind] = i - ind
            stack.append(i)
        return res
```
