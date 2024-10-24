# 代码随想录 Day48

### [图论基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

### [深度优先搜索基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E6%B7%B1%E6%90%9C%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#dfs-%E4%B8%8E-bfs-%E5%8C%BA%E5%88%AB)

### [广度优先搜索基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E5%B9%BF%E6%90%9C%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)

## 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

属于一道模板题，用 DFS 来回溯所有的可能性。

```
class Solution:
    def __init__(self):
        self.res = []
        self.path = [0]

    def dfs(self, graph, x):
        if x == len(graph)-1:
            self.res.append(self.path.copy())
            return

        for j in graph[x]:
            self.path.append(j)
            self.dfs(graph, j)
            self.path.pop()

    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        self.dfs(graph, 0)
        return self.res 
```
