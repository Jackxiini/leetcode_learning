# 代码随想录 Day55

### [prim算法精讲](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-prim.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)

关键想法是，每次都找能以最短距离到达 mst （最小生成树）的节点，加入到 mst 中。

[Video](https://www.youtube.com/watch?v=GLlIaT_PxVE)

### [kruskal算法精讲](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-Kruskal.html)

把每一个节点先当成一颗独立的树，将边按照权重从小到大排列，遍历排列，如果边的两个节点不在同一个树中，那么就加入树，否则抛弃，直到树的节点数等于总节点数。

[Video](https://www.youtube.com/watch?v=Z4jm4o2bt28)

## 1135. [Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)

和 [KC 53](https://kamacoder.com/problempage.php?pid=1053) 很像。找到能够通往所有城市的最小路径。

prim 和 kruskal 的模板题。注意 kruskal 还是要用到并查集，因为要检测两个节点在结果集中是否已经存在通路。

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

```
## kruskal
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def is_same(self, x, y):
        return self.find(x)==self.find(y)
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if not self.is_same(root_x, root_y):
            if self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            elif self.rank[root_y] > self.rank[root_x]:
                self.parent[root_x] = root_y
            else:
                self.rank[root_x] += 1
                self.parent[root_y] = root_x
            return True
        return False

class Solution:
    def minimumCost(self, n: int, connections: List[List[int]]) -> int:
        connections.sort(key=lambda x:x[2])
        uf = UnionFind(n+1)
        res = 0
        edge_count = 0
        for x, y, cost in connections:
            if not uf.is_same(x, y):
                res+=cost
                edge_count += 1
            uf.union(x, y)
        return res if edge_count == n-1 else -1
```
