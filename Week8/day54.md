# 代码随想录 Day54

## 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

[Course Link](https://www.programmercarl.com/kamacoder/0108.%E5%86%97%E4%BD%99%E8%BF%9E%E6%8E%A5.html#%E6%80%9D%E8%B7%AF)

和 [KC 108](https://kamacoder.com/problempage.php?pid=1181) 非常像的题。

用并查集可以轻松解，把所有能合并的都合并了，如果最后还检测到了两个节点在同一个集合内，那么表明他们两个形成了环。

```
class Unionfind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def is_same(self, x, y):
        return self.find(x) == self.find(y)
    
    def join(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = self.parent[root_y]
        elif self.rank[root_y] < self.rank[root_x]:
            self.parent[root_y] = self.parent[root_x]
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1

class Solution:     
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        uf = Unionfind(len(edges)+1)
        for x, y in edges:
            if uf.is_same(x, y):
                return [x, y]
            uf.join(x, y)
```

## 685. [Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/)

[Course Link](https://www.programmercarl.com/kamacoder/0109.%E5%86%97%E4%BD%99%E8%BF%9E%E6%8E%A5II.html)

和 [KC 109](https://kamacoder.com/problempage.php?pid=1182) 非常像的题。

很难，不太明白，二刷再看。

```
class Unionfind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # 路径压缩
        return self.parent[x]

    def join(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return False  # 返回False表示合并失败，即已存在环

        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_y] < self.rank[root_x]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1

        return True

class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        uf = Unionfind(n + 1)
        parent = list(range(n + 1))
        edge1, edge2 = None, None

        # Step 1: Check for nodes with two parents
        for u, v in edges:
            if parent[v] != v:
                edge1 = [parent[v], v]  # First edge that causes double parent
                edge2 = [u, v]          # Second edge that causes double parent
            else:
                parent[v] = u

        # Step 2: Union-Find to detect a cycle
        for u, v in edges:
            if edge2 and [u, v] == edge2:
                continue  # Skip edge2 if double parent case exists
            if not uf.join(u, v):
                # Case 1: No double parent case, we found a cycle
                if not edge2:
                    return [u, v]
                # Case 2: Double parent case and we found a cycle
                return edge1

        # Case 3: No cycle found, return the second edge of the double parent
        return edge2
```
