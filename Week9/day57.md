# 代码随想录 Day57

### [拓扑排序](https://www.programmercarl.com/kamacoder/0117.%E8%BD%AF%E4%BB%B6%E6%9E%84%E5%BB%BA.html#%E6%8B%93%E6%89%91%E6%8E%92%E5%BA%8F%E7%9A%84%E8%83%8C%E6%99%AF)

拓扑排序也可以判断图中是否有环

## 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

经典题，先上 b 课才能上 a 课，求一个上课顺序。用 Kahn’s Algorithm (BFS) 即可解。

```
from collections import defaultdict, deque

class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = defaultdict(list)
        indegree = [0] * (numCourses)
        
        for pre, end in prerequisites:
            graph[pre].append(end)
            indegree[end] += 1
        
        res = []
        que = deque([i for i in range(numCourses) if indegree[i]==0])

        while que:
            course = que.pop()
            res.append(course)

            for post in graph[course]:
                indegree[post] -= 1
                if indegree[post] == 0:
                    que.appendleft(post)
        
        return res[::-1] if len(res) == numCourses else []
```

### [Dijkstra算法](https://www.programmercarl.com/kamacoder/0047.%E5%8F%82%E4%BC%9Adijkstra%E6%9C%B4%E7%B4%A0.html#%E6%80%9D%E8%B7%AF)

## 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

依旧是经典 Dijstra 算法题。

```
import heapq
from collections import defaultdict

class Solution:
    def __init__(self):
        self.adj = defaultdict(list)
    
    def dijkstra(self, source, n, distance):
        pq = [(0, source)]
        distance[source] = 0

        while pq:
            dist, cur_node = heapq.heappop(pq)
            if dist > distance[cur_node]:
                continue
            for neighbor_dist, neighbor in self.adj[cur_node]:
                if neighbor_dist + dist < distance[neighbor]:
                    distance[neighbor] =  neighbor_dist + dist
                    heapq.heappush(pq, (distance[neighbor], neighbor))


    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        for start, end, dist in times:
            self.adj[start].append((dist, end))
        
        distance = [float('inf')] * (n+1)
        self.dijkstra(k, n, distance)

        res = max(distance[1:])
        return -1 if res==float('inf') else res
```
