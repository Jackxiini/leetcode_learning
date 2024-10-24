# 代码随想录 Day50

## 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

深度搜索 [Course Link](https://www.programmercarl.com/kamacoder/0099.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%95%B0%E9%87%8F%E6%B7%B1%E6%90%9C.html#%E6%80%9D%E8%B7%AF)
广度搜索 [Course Link](https://www.programmercarl.com/kamacoder/0099.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%95%B0%E9%87%8F%E5%B9%BF%E6%90%9C.html)

可以用广搜，也可以用深搜。

用 dfs，遍历时只要遇到 1 ，计数就 +1，并且进入 dfs，沿着一条路，把一路上所有的点都标记为 0 （因为它们都是被连接着的），全递归完了才退出 dfs，然后遍历进入下一个点。

用 bfs，就要用到栈，把当前的值变成 0，再把上下左右推入栈，这样 pop 栈的时候可以按次序把周围的都给遍历一遍。

这两个其实和之前树的 dfs 和 bfs 是非常像的。

```
## DFS
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def dfs(i ,j):
            if i >= len(grid) or j >= len(grid[0]) or i < 0 or j < 0 or grid[i][j] == '0':
                return
            grid[i][j] = '0'
            dfs(i+1, j)
            dfs(i-1, j)
            dfs(i, j+1)
            dfs(i, j-1)

        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    count += 1
                    dfs(i, j)
        return count
```

```
## BFS
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        directions = [(0, -1), (0, 1), (1, 0), (-1, 0)]
        row, col = len(grid), len(grid[0])
        def bfs(i, j):
            que = deque([(i, j)])
            while que:
                x, y = que.popleft()
                grid[x][y] = '0'
                for dx, dy in directions:
                    new_x = dx + x
                    new_y = dy + y
                    if 0<=new_x<row and 0<=new_y<col and grid[new_x][new_y] == '1':
                        grid[new_x][new_y] = '0'
                        que.append((new_x, new_y))

        count = 0
        for i in range(row):
            for j in range(col):
                if grid[i][j] == '1':
                    count += 1
                    bfs(i, j)
        return count
```

## 695. [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

[Course Link](https://www.programmercarl.com/kamacoder/0100.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%9C%80%E5%A4%A7%E9%9D%A2%E7%A7%AF.html)

和之前的几乎是一样的，这次要找到最大面积的岛，那么依旧是找到 1 就 bfs，求出当前岛的面积，最后取最大。

```
from collections import deque

class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        directions = [(0, -1), (0, 1), (1, 0), (-1, 0)]
        rows, cols = len(grid), len(grid[0])
        def bfs(i, j):
            area = 1
            que = deque([(i, j)])
            while que:
                x, y = que.popleft()
                grid[x][y] = 0
                for dx, dy in directions:
                    new_x = dx + x
                    new_y = dy + y
                    if 0<=new_x<rows and 0<=new_y<cols and grid[new_x][new_y] == 1:
                        area += 1
                        grid[new_x][new_y] = 0
                        que.append((new_x, new_y))
            return area
        
        max_area = 0
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    area = bfs(i, j)
                    max_area = max(max_area, area)
        return max_area
```
