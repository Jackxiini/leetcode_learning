# 代码随想录 Day54

### [prim算法精讲](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-prim.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)

关键想法是，每次都找能以最短距离到达 mst （最小生成树）的节点，加入到 mst 中。

[video](https://www.youtube.com/watch?v=GLlIaT_PxVE)

### [kruskal算法精讲](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-Kruskal.html)

## 1135. [Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)

和 [KC 53](https://kamacoder.com/problempage.php?pid=1053) 很像。找到能够通往所有城市的最小路径。

prim 和 kruskal 的模板题

```
## prim
from collections import defaultdict
import heapq

class Solution:
    def minimumCost(self, n: int, connections: List[List[int]]) -> int:
        grid = defaultdict(list)
        for x, y, cost in connections:
            grid[x].append((cost, y))
            grid[y].append((cost, x))
        
        visited = [False] * (n+1)
        min_heap = [(0, 1)]
        mst_cost = 0
        nodes_in_mst = 0

        while min_heap and nodes_in_mst < n:
            cost, u = heapq.heappop(min_heap)
            if visited[u]:
                continue
            visited[u] = True
            mst_cost += cost
            nodes_in_mst += 1

            for edge_cost, v in grid[u]:
                if not visited[v]:
                    heapq.heappush(min_heap, (edge_cost, v))
        
        return mst_cost if nodes_in_mst == n else -1
```
