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

## 827. [Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

[Course Link](https://www.programmercarl.com/kamacoder/0104.%E5%BB%BA%E9%80%A0%E6%9C%80%E5%A4%A7%E5%B2%9B%E5%B1%BF.html#%E6%80%9D%E8%B7%AF)

选择一个有水的点变成陆地，使得变后的最大岛屿最大。思路是先遍历地图并计算每个岛屿的面积是多少，并且储存。可以找到一个岛屿后，把所有的 1 变成 大于 1 的数，以便标记区分岛屿。然后再遍历一遍地图，计算把当前点变成陆地后 上下左右 的岛加起来，总面积是多少，取最大值就是答案。

要注意加上变成陆地后的这个点的面积（也就是1），还要注意用一个 set 去记录已经计算过的岛屿，最后还要注意如果没有水的情况（也就是整张地图就是一个岛）

```
from collections import deque
class Solution:
    def largestIsland(self, grid: List[List[int]]) -> int:
        directions = [(-1, 0), (1, 0), (0, 1), (0, -1)]
        rows, cols = len(grid), len(grid[0])
        areas = {}
        num = 1
        max_area = 0

        def bfs(i, j, num):
            que = deque([(i, j)])
            count = 1
            while que:
                x, y = que.pop()
                grid[x][y] = num
                for dx, dy in directions:
                    new_x = dx + x
                    new_y = dy + y
                    if 0<=new_x<rows and 0<=new_y<cols and grid[new_x][new_y] == 1:
                        count += 1
                        grid[new_x][new_y] = num
                        que.append((new_x, new_y))
            return count
        
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    num += 1
                    areas[num] = bfs(i, j, num)

        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 0:
                    area = 1  #the area of the current point
                    v = set()  #record which island we already added
                    for dx, dy in directions:
                        x = dx + i
                        y = dy + j
                        if 0<=x<rows and 0<=y<cols and grid[x][y] != 0 and grid[x][y] not in v:
                            area += areas[grid[x][y]]
                            v.add(grid[x][y])
                    max_area = max(area, max_area)

        if max_area == 0: #if there is no water
            return areas[2]
        
        return max_area
```
