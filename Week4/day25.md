# 代码随想录 Day25

## 491. [Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)

[Course Link](https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html)

根据题意不能给 input 先排序，然后又得去重，所以得使用 used 数组记录当前层用过的数。另外，输出的子数组都得是正序，所以得在递归层面检查是否比上一层的数大（或等于）

```
class Solution:
    def backtracking(self, nums, res, comb, start, pre ,used):
        if len(comb) > 1:
            res.append(comb[:])

        for i in range(start, len(nums)):
            if pre > nums[i] or nums[i] in used:
                continue
            comb.append(nums[i])
            used.add(nums[i])
            self.backtracking(nums, res, comb, i+1, nums[i], set())
            comb.pop()

    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, res, [], 0, float('-inf'), set())
        return res
```

## 46. [Permutations](https://leetcode.com/problems/permutations/)

[Course Link](https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

求排列，并且 input 里没有重复元素。只需要跳过已经用过的数，别的都一样。

```
class Solution:
    def backtracking(self, nums, res, comb):
        if len(comb) == len(nums):
            res.append(comb[:])
            return
        
        for i in range(len(nums)):
            if nums[i] in comb:
                continue
            comb.append(nums[i])
            self.backtracking(nums, res, comb)
            comb.pop()

    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        self.backtracking(nums, res, [])
        return res
```

## 47. [Permutations II](https://leetcode.com/problems/permutations-ii/)

[Course Link](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)

因为有重复数字了所以不能用 LC 46 里面那种检查是否出现过的写法了。

链接里还有另一种写法，这里贴上自己的写法，每次传入 nums 的数组都是去除这一次使用过的数的数组，但是要提前记录一下原始 nums 的长度，来保证终止条件里的长度是原始长度（用 len(nums) 是错误的，因为长度会变）

```
class Solution:
    def backtracking(self, nums, res, comb, used, leng):
        if len(comb) == leng:
            res.append(comb[:])
            return 
        
        for i in range(len(nums)):
            if nums[i] in used:
                continue
            comb.append(nums[i])
            used.add(nums[i])
            self.backtracking(nums[:i]+nums[i+1:], res, comb, set(), leng)
            comb.pop()

    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res = []
        leng = len(nums)
        self.backtracking(nums, res, [], set(), leng)
        return res
```

## 332. [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

[Course Link](https://programmercarl.com/0332.%E9%87%8D%E6%96%B0%E5%AE%89%E6%8E%92%E8%A1%8C%E7%A8%8B.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

第一眼看上去是懵逼的，想不到回溯能怎么解决这个问题。
回头有空再看一下解法

```
from collections import defaultdict
class Solution:
    def __init__(self):
        self.route = defaultdict(list)

    def dfs(self, depart, res):
        while depart in self.route and len(self.route[depart]) > 0:
            arriv = self.route[depart][0]
            self.route[depart].pop(0)
            self.dfs(arriv, res)
        res.append(depart)

    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        tickets.sort(key=lambda x:x[1])
        for depart, arriv in tickets:
            self.route[depart].append(arriv)
        
        res = []
        self.dfs('JFK', res)
        
        return res[::-1]
```

# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

[Course Link](https://programmercarl.com/0051.N%E7%9A%87%E5%90%8E.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E8%A1%A5%E5%85%85)

N皇后问题，每次操作一行，摆一个皇后，然后检查棋盘上是否有冲突，如果有那么往后挪位子继续试，直到摆完N个皇后，也没有冲突，才放进结果集。
自己想明白写了一遍，和链接里的思路一样但是写法有点区别。再刷可以精进一下。

```
class Solution:
    def isValid(self, index, n, board):
        if not board:
            return True
        n_row = len(board)
        for i in range(n_row):
            if board[n_row-i-1][index] == 'Q':
                return False
            if index-i-1 >=0 and board[n_row-i-1][index-i-1] == 'Q':
                return False
            if index+i+1 < n and board[n_row-i-1][index+i+1]  == 'Q':
                return False
        return True

    def backtracking(self, n, row, res, board):
        if row == n:
            res.append(board[:])
            return

        for col in range(n):
            if self.isValid(col, n, board):
                cur_row = ['.']*n
                cur_row[col] = 'Q'
                board.append(''.join(cur_row))
                self.backtracking(n, row+1, res, board)
                board.pop()

    def solveNQueens(self, n: int) -> List[List[str]]:
        res = []
        self.backtracking(n, 0, res, [])
        return res
```

## 37. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

[Course Link](https://programmercarl.com/0037.%E8%A7%A3%E6%95%B0%E7%8B%AC.html#%E6%80%9D%E8%B7%AF)

解数独，很难，关键是需要用二维递归，遍历 row 再遍历 column，然后递归，而且要注意调用递归函数要返回 bool（而且只有一个答案，不需要记录不同的答案），这样才能知道这一组是否找到了合适的值，从而回溯。

```
class Solution:
    def isValid(self, board, row, col, val):
        for i in range(9): # check row and col
            if board[row][i] == str(val):
                return False
            if board[i][col] == str(val):
                return False
        block_row = row // 3
        block_col = col // 3
        for i in range(3*block_row, 3*block_row+3):
            for j in range(3*block_col, 3*block_col+3):
                if board[i][j] == str(val):
                    return False
        return True

    def backtracking(self, board):
        for row in range(9):
            for col in range(9):
                if board[row][col] != ".": 
                    continue
                for i in range(1,10):
                    if self.isValid(board, row, col, i): 
                        board[row][col] = str(i)
                        if self.backtracking(board): return True
                        board[row][col] = "."
                return False
        return True

        
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        self.backtracking(board)
```

虽然可以解决问题，但是貌似不是最优的解法，还能进一步提速，再刷再看。
