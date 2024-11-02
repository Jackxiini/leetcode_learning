# 代码随想录 Day59

### [队列优化 Bellman-Ford -- SPFA](https://www.programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I-SPFA.html#%E8%83%8C%E6%99%AF)

由于对当前节点未能接触到的地方进行更新都是无用功，因此我们跳过那些还不能接触到的节点，实现上可以用队列，类似于 BFS 的操作。

### [队列优化 Bellman-Ford 检测负权值环](https://www.programmercarl.com/kamacoder/0095.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93II.html#%E6%80%9D%E8%B7%AF)

有多种方法检测，非队列优化版本的检测可以看昨天的 code，队列优化的版本可以看 KC 95。

## KC 94. [城市间货物运输 I](https://kamacoder.com/problempage.php?pid=1152)

图里没有负权值环的情况

```
from collections import deque

def bellman_ford(src, graph, n):
    dist = [float('inf')] * (n+1)
    dist[src] = 0
    que = deque([src])
    in_que = [False] * (n+1)
    in_que[src] = True
    
    while que:
        u = que.popleft()
        in_que[u] = False
        for v, weight in graph[u]:
            if dist[u] != float('inf'):
                dist[v] = min(dist[v], dist[u]+weight)
                if not in_que[v]:
                    in_que[v] = True
                    que.append(v)
    
    return dist
    
if __name__ == "__main__":
    n, m = map(int, input().strip().split())
    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, weight = map(int, input().strip().split())
        graph[u].append((v, weight))
    
    dist = bellman_ford(1, graph, n)
    if dist:
        if dist[-1] != float('inf'):
            print(dist[-1])
        else:
            print("unconnected")
```

## KC 95. [城市间货物运输 II](https://kamacoder.com/problempage.php?pid=1153)

图里有负权值环的情况，需要判断是否有负权值。可以用一个数组（relax_count）记录每个节点松弛的次数，松弛次数顶多是 n - 1次，超过了就说明有环。

```
from collections import deque

def bellman_ford(src, edges, n):
    dist = [float('inf')] * (n+1)
    dist[src] = 0
    que = deque([src])
    in_que = [False] * (n+1)
    in_que[src] = True
    
    relax_count = [0] * (n+1)
    
    while que:
        u = que.popleft()
        in_que[u] = False
        for v, weight in edges[u]:
            if dist[u] != float('inf'):
                dist[v] = min(dist[v], dist[u]+weight)
                
                relax_count[v] += 1
                if relax_count[v] > n - 1:
                    return "circle"
                    
                if not in_que[v]:
                    que.append(v)
                    in_que[v] = True
    
    return dist
    
if __name__ == "__main__":
    n, m = map(int, input().strip().split())
    edges = [[] for _ in range(n+1)]
    for _ in range(m):
        u, v, weight = map(int, input().strip().split())
        edges[u].append((v, weight))
    
    dist = bellman_ford(1, edges, n)
    if dist == "circle":
        print("circle")
    elif dist[-1] == float('inf'):
        print('unconnected')
    else:
        print(dist[-1])
```

## KC 96. [城市间货物运输 III](https://kamacoder.com/problempage.php?pid=1154)

[Course Link](https://www.programmercarl.com/kamacoder/0096.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93III.html#%E6%80%9D%E8%B7%AF)

最多只能经过 k 个城市，而不是之前的无限制。

如果没有限制，那么我们相当于是松弛了 n - 1 次后的结果（如果有 n 个节点，那么 n - 1 条边，对所有边松弛一次，相当于计算 起点到达 与起点一条边相连的节点 的最短距离，那么对所有边松弛 k + 1次，就是求 起点到达 与起点k + 1条边相连的节点的 最短距离）。这题感觉有点文字游戏了，经过 k 个城市，所以是松弛了 k + 1 次边（起点不算经过，到达也不算）。

但这里要注意的是，我们不能简单的将循环的 n - 1 改成 k + 1 了事，因为每次循环时，当前节点的计算会依赖于上一个节点计算（同一个循环内的），而这对于本题的限制是不行的，所以我们需要记录一下上一个循环的 dist 表。

```
def bellman_ford(src, k, edges, n):
    dist = [float('inf')] * (n+1)
    dist[src] = 0
    
    for _ in range(k + 1):
        pre_dist = dist[:]
        for u, v, weight in edges:
            if pre_dist[u] != float('inf') and dist[v] > pre_dist[u] + weight:
                dist[v] = pre_dist[u] + weight
    
    return dist
        
    
if __name__ == "__main__":
    n, m = map(int, input().strip().split())
    edges = []
    for _ in range(m):
        u, v, weight = map(int, input().strip().split())
        edges.append((u, v, weight))
        
    src, dst, k = map(int, input().strip().split())
    
    dist = bellman_ford(src, k, edges, n)
    
    if dist[dst] == float('inf'):
        print("unreachable")
    else:
        print(dist[dst])
```

SPFA 法同样可以解这个，但是我这里没做，可以看链接学习。
