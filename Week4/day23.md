# 代码随想录 Day23

## 39. [Combination Sum](https://leetcode.com/problems/combination-sum/)

[Course Link](https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html)

和 LC 77 有点像但是数可以重复使用，并且数组不是有序的。自己写了一遍发现要注意更新 start index，例如 [2,2,3] 这个数组，如果不限制遍历往后行进了，就不能再用之前（start index之前）的数了，就会出现 [2,3,2] 这种重复的结果出现

```
class Solution:
    def backtracking(self, candidates, target, res, comb, cur_sum, start):
        if cur_sum == target:
            res.append(comb[:])
            return 
        
        for i in range(start, len(candidates)):
            cur_sum += candidates[i]
            if cur_sum > target:
                return
            comb.append(candidates[i])
            self.backtracking(candidates, target, res, comb, cur_sum, i)
            comb.pop()
            cur_sum -= candidates[i]

    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        candidates.sort()
        self.backtracking(candidates, target, res, [], 0, 0)
        return res
```

## 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

[Course Link](https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html)

这题的关键是要去重，题目要求结果集里不能有重复的组合，例如 [1,1,2,2,5] 里去找和为 8 的组合，我们会找到好几个 [1,2,5] 因为数组中有多个 1 和 2，但是结果只能保留一组 [1,2,5]。

如果从树结构（根据 Course Link 里教的画树）来看，那么每一层不能重复取相同的数，所以在循环里如果碰到现在的数和之前的数是一样的，那么就跳过这一轮

```
class Solution:
    def backtracking(self, candidates, target, res, comb, start, total):
        if total == target:
            res.append(comb[:])
            return

        for i in range(start, len(candidates)):
            if i > start and candidates[i-1] == candidates[i]:
                continue
            total += candidates[i]
            if total > target:
                break
            comb.append(candidates[i])
            self.backtracking(candidates, target, res, comb, i+1, total)
            comb.pop()
            total -= candidates[i]

    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        candidates.sort()
        self.backtracking(candidates, target, res, [], 0, 0)
        return res
```

## 131. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

[Course Link](https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

学会用回溯来做切割问题，关键是使用一个 index （这里我叫 split）来记录切割点，切割完后，下一层的操作就在切割以后的字符串上做切割了。

这里判断回文可以用 s == s[::-1]，但是空间复杂度是 O(n)，可以写一个函数来判断，空间复杂度降为 O(1)。但是它们俩时间复杂度都是 O(n)。

```
class Solution:
    def palindrome(self, s):
        left, right = 0, len(s)-1
        while left < right:
            if s[left] != s[right]:
                return False
            left+=1
            right-=1
        return True

    def backtracking(self, s, res, comb, split):
        if split == len(s):
            res.append(comb[:])
            return
        
        for i in range(split, len(s)):
            if not self.palindrome(s[split:i+1]):
                continue
            comb.append(s[split:i+1])
            self.backtracking(s, res, comb, i+1)
            comb.pop()

    def partition(self, s: str) -> List[List[str]]:
        res = []
        self.backtracking(s, res, [], 0)
        return res
```
