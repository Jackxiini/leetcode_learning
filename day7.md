# 代码随想录 Day7

## [4Sum II](https://leetcode.com/problems/4sum-ii/)

[Course Link](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

defaultdict 在字典里没有相应 key 的时候会以默认 type （这里是int） 创建 key: value对，而不像用 dict() 会报错，因此更高效一些

```
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        from collections import defaultdict
        record1 = defaultdict(int)
        count = 0
        for i in nums1:
            for j in nums2:
                record1[i+j] += 1
        for i in nums3:
            for j in nums4:
                tmp = 0-i-j
                if tmp in record1.keys():
                    count += record1[tmp]
        return count
```
