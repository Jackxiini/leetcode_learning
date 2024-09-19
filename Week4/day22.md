# 代码随想录 Day22

## 77. [Combinations](https://leetcode.com/problems/combinations/)

[Course Link](https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html#%E5%89%AA%E6%9E%9D%E4%BC%98%E5%8C%96)

组合经典题，用回溯算法，先自己写了一遍

```
class Solution:
    def __init__(self):
        self.res = []
        self.comb = []

    def backtracking(self, cand, k):
        if len(self.comb)==k:
            self.res.append(self.comb[:])
            return
            
        for i in range(len(cand)):
            self.comb.append(cand[i])
            self.backtracking(cand[i+1:], k)
            self.comb.pop()

    def combine(self, n: int, k: int) -> List[List[int]]:
        cand = [i for i in range(1, n+1)]
        self.backtracking(cand, k)
        return self.res
```

实际上不用事先创建数组，可以通过一个 start index 来记录遍历到哪了

```
class Solution:
    def backtracking(self, n, k, res, start, comb):
        if len(comb) == k:
            res.append(comb[:])
            return
        for i in range(start, n+1):
            comb.append(i)
            self.backtracking(n, k, res, i+1, comb)
            comb.pop()

    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        self.backtracking(n, k, res, 1, [])
        return res
```

可以参考 course link 里的树结构来发现其实可以剪枝，把无用的遍历都给去除。

规则是从起始到 n+1-(还需要的个数），也就是 n+1-(k-(len(comb)+1))，这里 len(comb) 要加一因为遍历的此刻 i 还没有被 append 进 comb。最后算一下可以简化到 n-k+len(comb)+2

```
class Solution:
    def backtracking(self, n, k, res, start, comb):
        if len(comb) == k:
            res.append(comb[:])
            return
        for i in range(start, n-k+len(comb)+2): ## here for pruning
            comb.append(i)
            self.backtracking(n, k, res, i+1, comb)
            comb.pop()

    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        self.backtracking(n, k, res, 1, [])
        return res
```

## 216. [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

[Course Link](https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

和77基本差不多的写法，自己写了一遍

```
class Solution:
    def backtracking(self, k, n, comb, res, start):
        if sum(comb) == n and len(comb) == k:
            res.append(comb[:])
            return

        for i in range(start, 10):
            if sum(comb) + i > n and len(comb) == k:
                return
            comb.append(i)
            self.backtracking(k, n, comb, res, i+1)
            comb.pop()

    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        self.backtracking(k, n, [], res, 1)
        return res
```

也可以在循环的时候就把数量大于 k 的情况去除

```
class Solution:
    def backtracking(self, k, n, comb, res, start, cur_sum):
        if len(comb) == k:
            if cur_sum == n:
                res.append(comb[:])
            return

        for i in range(start, 10-(k-(len(comb)+1))):
            cur_sum += i
            if cur_sum > n:
                return
            comb.append(i)
            self.backtracking(k, n, comb, res, i+1, cur_sum)
            cur_sum -= i
            comb.pop()

    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        self.backtracking(k, n, [], res, 1, 0)
        return res
```

## 17. [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

[Course Link](https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

用 index 来记录遍历第几个数字了，使得回溯可以遍历多个 for 循环

```
class Solution:
    def __init__(self):
        self.letterMap = [
            "",     # 0
            "",     # 1
            "abc",  # 2
            "def",  # 3
            "ghi",  # 4
            "jkl",  # 5
            "mno",  # 6
            "pqrs", # 7
            "tuv",  # 8
            "wxyz"  # 9
        ]
        self.res = []
        self.s = ""

    def backtracking(self, digits, index):
        if index == len(digits):
            self.res.append(self.s)
            return
            
        digit = int(digits[index])
        letter = self.letterMap[digit]
        for i in range(len(letter)):
            self.s += letter[i]
            self.backtracking(digits, index+1)
            self.s = self.s[:-1]

    def letterCombinations(self, digits: str) -> List[str]:
        if len(digits) == 0:
            return self.res
        self.backtracking(digits, 0)
        return self.res
```
