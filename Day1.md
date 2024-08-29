# 代码随想录 Day1

## 704. [Binary Search](https://leetcode.com/problems/binary-search/)

[Course link](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#%E6%80%9D%E8%B7%AF)

两种写法：while (left <= right) 和 while (left < right)，前者左闭右闭，需要left=mid+1, right=mid-1，因为middle一定不是target，后者左闭右开，所以right得等于middle。

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)-1
        while left <= right:
            mid = (left+right)//2
            if nums[mid] < target:
                left = mid+1
            elif nums[mid] > target:
                right = mid-1
            else:
                return mid
        return -1
```

## 35. [Search Insert Position](https://leetcode.com/problems/search-insert-position/)

用二分法

```
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)-1
        while left <= right:
            mid = (left + right) //2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        if target > nums[mid]:
            return mid+1
        else:
            return mid
```

## 34. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

用二分法，这个还不是很熟练，还得再想想

```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def leftborder(nums, target):
            leftborder = -2
            left, right = 0, len(nums)-1
            while left<=right:
                mid = (left+right)//2
                if nums[mid] < target:
                    left = mid +1
                else:
                    right = mid - 1
                    leftborder = mid
            return leftborder
        
        def rightborder(nums, target):
            rightborder = -2
            left, right = 0, len(nums)-1
            while left<=right:
                mid = (left+right)//2
                if nums[mid] > target:
                    right=mid-1
                else:
                    left=mid+1
                    rightborder = mid
            return rightborder

        right = rightborder(nums, target)
        left = leftborder(nums, target)
        if right == -2 or left == -2:
            return [-1, -1]
        if right - left >= 0:
            return [left, right]
        return [-1, -1]
```

# 27. [Remove Element](https://leetcode.com/problems/remove-element/)

双指针: [Course link](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)
用快慢指针，快指针遍历全list，慢指针仅在快指针的数值不等于val的时候前进，以保存非val的数值以及不是val的list长度

```
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow=fast=0
        while fast < len(nums):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow+=1
            fast+=1
        return slow
```

# 977. [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

双指针

```
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        left, right = 0, len(nums)-1
        res = []
        while left <= right:
            left_val = nums[left] ** 2
            right_val = nums[right] ** 2
            if left_val < right_val:
                res.append(right_val)
                right -= 1
            else:
                res.append(left_val)
                left += 1
        return res[::-1]
        
```
