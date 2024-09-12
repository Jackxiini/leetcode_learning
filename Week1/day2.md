# 代码随想录 Day2

## 209. [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

[Course link](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

双指针法，快慢指针。写的比代码随想录里的繁琐一些，但是是一个道理。

```
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        summ = nums[0]
        slow, fast = 0, 0
        min_len = float('inf')
        while fast < len(nums):
            if summ < target:
                fast+=1
                if fast <= len(nums) -1:
                    summ+=nums[fast]
            else:
                cur_len = fast-slow+1
                if min_len > cur_len:
                    min_len = cur_len
                summ-=nums[slow]
                slow+=1
        return min_len if min_len != float('inf') else 0
```

## 59. [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

[Course link](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.htm)

很容易写错，多加几个test case多试试，range的区间要多模拟一下，第一圈可能没问题但是第二圈可能绕错（比如range少减个i会导致结果是不对的）

```
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        res = [[0]*n for _ in range(n)]
        rounds = n // 2
        cur = 1
        for i in range(rounds):
            for j in range(i, n-1-i):
                res[i][j] = cur
                cur+=1
            for j in range(i, n-1-i):
                res[j][n-1-i] = cur
                cur+=1
            for j in range(n-1-i, i, -1):
                res[n-1-i][j] = cur
                cur+=1
            for j in range(n-1-i, i, -1):
                res[j][i] = cur
                cur+=1
        if n % 2 != 0:
            res[rounds][rounds] = cur 
        return res
```
