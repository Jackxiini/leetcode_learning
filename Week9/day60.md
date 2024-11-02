# 代码随想录 Day60

### [Floyd 算法](https://www.programmercarl.com/kamacoder/0097.%E5%B0%8F%E6%98%8E%E9%80%9B%E5%85%AC%E5%9B%AD.html)

解多源最短路径问题，即我们的起点和终点都可以是多个，相比于之前的 Dijkstra（不能有负权路） 和 Bellman-Ford（可以有负权路），它们时单源最短路径，起点终点固定。

[Video](https://www.youtube.com/watch?v=XzmTiO3j6p0)

## KC 97. [小明逛公园](https://kamacoder.com/problempage.php?pid=1155)

注意是双向路，因此构建图的时候要两个方向都构建。Video 里的方法会创建一个 path 数组来记录两点的中转点，这里不需要写出路径，只要距离，所以不需要建立 path。

```
def floyd(grid, n):
    for k in range(1, n+1):
        for i in range(1, n+1):
            for j in range(1, n+1):
                if grid[i][j] > grid[i][k] + grid[k][j]:
                    grid[i][j] = grid[i][k] + grid[k][j]
    return grid
    
if __name__ == "__main__":
    n, m = map(int, input().strip().split())
    grid = [[float('inf')]*(n+1) for _ in range(n+1)]
    
    for _ in range(m):
        u, v, w = map(int, input().strip().split())
        grid[u][v] = w
        grid[v][u] = w
        
    grid = floyd(grid, n)
    
    z = int(input())
    for _ in range(z):
        start, end = map(int, input().strip().split())
        if grid[start][end] != float('inf'):
            print(grid[start][end])
        else:
            print(-1)
```

### [A* 算法](https://www.programmercarl.com/kamacoder/0126.%E9%AA%91%E5%A3%AB%E7%9A%84%E6%94%BB%E5%87%BBastar.html)

完结！
