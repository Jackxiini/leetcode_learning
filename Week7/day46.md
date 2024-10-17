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

## 496. [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

[Course Link](https://programmercarl.com/0496.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0I.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

做了上一题后最直觉的想法就是先找到 nums2 的每个数的 next greater element，然后用 nums1 的数提取出 nums2 的结果作为结果。

```
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []
        res = [-1]*len(nums2)
        res1 = []
        for i in range(len(nums2)):
            while stack and nums2[i]>nums2[stack[-1]]:
                ind = stack.pop()
                res[ind] = nums2[i]
            stack.append(i)
        for i in nums1:
            res1.append(res[nums2.index(i)])
        return res1
```

简化一下

```
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []
        res = [-1]*len(nums1)
        for i in range(len(nums2)):
            while stack and nums2[i]>nums2[stack[-1]]:
                ind = stack.pop()
                if nums2[ind] in nums1:
                    index = nums1.index(nums2[ind])
                    res[index] = nums2[i]
            stack.append(i)
        return res
```

## 503. [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)

[Course Link](https://programmercarl.com/0503.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

和前两题如出一辙，区别就是这题的数组是环状的，最后一个数还可以从头循环找 next greater element。那么解决办法也非常直接，循环两次即可。

```
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        stack = []
        res = [-1]*len(nums)
        for i in range(len(nums)*2):
            while stack and nums[i%len(nums)] > nums[stack[-1]]:
                index = stack.pop()
                res[index] = nums[i%len(nums)]
            stack.append(i%len(nums))
        return res
```
