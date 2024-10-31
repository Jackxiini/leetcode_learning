# 代码随想录 Day58

### [Dijkstra算法堆优化](https://www.programmercarl.com/kamacoder/0047.%E5%8F%82%E4%BC%9Adijkstra%E5%A0%86.html#%E6%80%9D%E8%B7%AF)

不能带负权值，否则不能用 Dijkstra 算法。

### [Bellman-Ford算法](https://www.programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html)

带负权值的最短路径算法。图中不能有负权环，不然就可以不停的转，总权重越来越小。但好在算法可以检测到这个状态。

## KC 94. [城市间货物运输 I](https://kamacoder.com/problempage.php?pid=1152)

模板题，题目是说明一定没有负回路的，但是模板还是写一下比较好。最后还要注意给的图不一定能走得通（1 不一定一定可以到达 n），所以要判断是否通路。模板中不写 early termination（updated = False那个逻辑）会超时，写一下就不超时了。

```
def bellman_ford(src, graph, n):
    dist = [float('inf')] * (n+1)
    dist[src] = 0
    
    for _ in range(n-1):
        updated = False
        for v, u, weight in graph:
            if dist[v] != float('inf') and dist[v]+weight < dist[u]:
                dist[u] = dist[v]+weight
                updated = True
        
        if not updated:
            break
    
    #check negative cycles
    for v, u, weight in graph:
        if dist[v] != float('inf') and dist[v]+weight < dist[u]:
            print("Has negative cycles")
            return None
    
    return dist
    
if __name__ == "__main__":
    n, m = map(int, input().strip().split())
    graph = []
    for _ in range(m):
        v, u, weight = map(int, input().strip().split())
        graph.append([v, u, weight])
    
    dist = bellman_ford(1, graph, n)
    if dist:
        if dist[-1] != float('inf'):
            print(dist[-1])
        else:
            print("unconnected")
```
