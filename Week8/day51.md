# 代码随想录 Day51

## 1254. [Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/)

[Course Link](https://www.programmercarl.com/kamacoder/0101.%E5%AD%A4%E5%B2%9B%E7%9A%84%E6%80%BB%E9%9D%A2%E7%A7%AF.html#%E6%80%9D%E8%B7%AF)
[Course Link](https://www.programmercarl.com/kamacoder/0102.%E6%B2%89%E6%B2%A1%E5%AD%A4%E5%B2%9B.html#%E6%80%9D%E8%B7%AF)

Course Link 里两道题和这题是一样的。由于我们要找出四面环海的岛，所以靠地图边上的岛是不符合要求的。方法就是先遍历地图四边，将岛都变成海，然后再遍历一次整个地图，既可以找到符合要求的岛。

```
from collections import deque

class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

        def bfs(i, j):
            que = deque([(i, j)])
            while que:
                x, y = que.pop()
                grid[x][y] = 1
                for dx, dy in directions:
                    new_x = dx + x
                    new_y = dy + y
                    if 0<=new_x<rows and 0<=new_y<cols and grid[new_x][new_y] == 0:
                        grid[new_x][new_y] = 1
                        que.appendleft((new_x, new_y))
        
        for i in range(rows):
            if grid[i][0] == 0: bfs(i, 0)
            if grid[i][cols-1] == 0: bfs(i, cols-1)
        for j in range(cols):
            if grid[0][j] == 0: bfs(0, j)
            if grid[rows-1][j] == 0: bfs(rows-1, j)
        
        count = 0
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 0:
                    count+=1
                    bfs(i, j)
        return count
```
