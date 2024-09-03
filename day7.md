# 代码随想录 Day7

## 454. [4Sum II](https://leetcode.com/problems/4sum-ii/)

[Course Link](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

defaultdict 在字典里没有相应 key 的时候会以默认 type （这里是int） 创建 key: value对，而不像用 dict() 会报错，因此更高校一些

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

## 383. [Ransom Note](https://leetcode.com/problems/ransom-note/)

[Course Link](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)

解法很多，参考链接

```
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        from collections import Counter
        record = Counter(magazine)
        for i in ransomNote:
            if i in record.keys():
                record[i] -= 1
                if record[i] == 0:
                    del record[i]
            else:
                return False
        return True
```

## 15. [3Sum](https://leetcode.com/problems/3sum/)

[Course Link](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

需要注意去除重复元素，用双指针则需要先排序再跳过和前一个元素相同的元素

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums)):
            if i==0 or nums[i]!=nums[i-1]:
                need = 0 - nums[i]
                left, right = i+1, len(nums)-1
                while left < right:
                    sum_ = nums[left]+nums[right]
                    if sum_ < need:
                        left+=1
                    elif sum_ > need:
                        right-=1
                    else:
                        res.append([nums[i],nums[left],nums[right]])
                        left+=1
                        right-=1
                        while left < right and nums[left-1]==nums[left]:
                            left+=1
                        while left < right and nums[right+1]==nums[right]:
                            right-=1
        return res
```

## 18. [4Sum](https://leetcode.com/problems/4sum/)

[Course Link](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)

和上一题 3Sum 一个做法，但要多注意一层跳过重复元素。同样类型的题，再多个数（5Sum，6Sum。。。）都是一样的，只是时间复杂度会上升，3Sum是 O(n^2)，4Sum是 O(n^3)，以此类推。

```
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums)-2):
            if i>0 and nums[i-1]==nums[i]:
                continue
            for j in range(i+1, len(nums)-1):
                if j>i+1 and nums[j-1]==nums[j]:
                    continue
                left, right = j+1, len(nums)-1
                while left<right:
                    sum_ = nums[i]+nums[j]+nums[left]+nums[right]
                    if sum_>target:
                        right-=1
                    elif sum_<target:
                        left+=1
                    else:
                        res.append([nums[i],nums[j],nums[left],nums[right]])
                        left+=1
                        right-=1
                        while left<right and nums[left-1]==nums[left]:
                            left+=1
                        while left<right and nums[right+1]==nums[right]:
                            right-=1
        return res

```
