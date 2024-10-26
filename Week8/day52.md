# 代码随想录 Day52

## KC 110. [字符串接龙](https://kamacoder.com/problempage.php?pid=1183)

[Course Link](https://www.programmercarl.com/kamacoder/0110.%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%8E%A5%E9%BE%99.html#%E6%80%9D%E8%B7%AF)

利用 strList 中的 string 作为过渡，从 beginStr 过度到 endStr，每次只能变一个字符。

首先需要建立一个图，才能知道每个字符串能够连接到哪（和哪些字符串只差一个字符），然后用 bfs 找到最短路径，实现的时候不需要真的建立一个图，只需要用一个函数判断当前字符串是否和遍历的字符串相邻，并且要注意把见到过的字符串记录，防止无限循环。

```
def judge(s1, s2):
    count = 0
    for i in range(len(s1)):
        if s1[i] != s2[i]:
            count+=1
    return count==1

from collections import deque        
if __name__=='__main__':
    n=int(input())
    beginstr,endstr=map(str,input().split())
    if beginstr==endstr:
        print(0)
        exit()
    strlist=[]
    for i in range(n):
        strlist.append(input())
    
    que = deque([(beginstr, 1)])
    v = set(beginstr)
    while que:
        string, step = que.pop()
        if judge(string, endstr):
            print(step+1)
            exit()
        for cand in strlist:
            if judge(cand, string) and cand not in v:
                v.add(cand)
                que.appendleft((cand, step+1))
    
    print(0)
```

## KC 105. [有向图的完全可达性](https://kamacoder.com/problempage.php?pid=1177)

[Course Link](https://www.programmercarl.com/kamacoder/0105.%E6%9C%89%E5%90%91%E5%9B%BE%E7%9A%84%E5%AE%8C%E5%85%A8%E5%8F%AF%E8%BE%BE%E6%80%A7.html)

用广搜深搜都可以。这里习惯性用广搜，先要简历一个图，然后广搜记录走过的路径，如果全都走到了，那么就是 True 不然 False

```
import collections

def bfs(src, graph):
    path = set()
    que = collections.deque([src])
    while que:
        cur = que.pop()
        path.add(cur)
        for neighbor in graph[cur]:
            que.appendleft(neighbor)
        graph[cur] = []
    return path
    
def main():
    N, K = map(int, input().strip().split())
    graph = collections.defaultdict(list)
    for _ in range(K):
        src, dest = map(int, input().strip().split())
        graph[src].append(dest)
    path = bfs(1, graph)
    if len(path) == len(graph):
        return 1
    return -1        

if __name__ == "__main__":
    print(main())
```

## KC 106. [岛屿的周长](https://kamacoder.com/problempage.php?pid=1178)

[Course Link](https://www.programmercarl.com/kamacoder/0106.%E5%B2%9B%E5%B1%BF%E7%9A%84%E5%91%A8%E9%95%BF.html)

题比较特别，根本不需要 bfs，dfs 就可以解。只需要遍历到 1 的时候，上下左右逢 0 周长 +1，如果在边上，那么也 +1

```
def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    # 读取 n 和 m
    n = int(data[0])
    m = int(data[1])
    
    # 初始化 grid
    grid = []
    index = 2
    for i in range(n):
        grid.append([int(data[index + j]) for j in range(m)])
        index += m
    
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    count = 0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == 1:
                for dx, dy in directions:
                    if 0<=i+dx<len(grid) and 0<=j+dy<len(grid[0]):
                        if grid[dx+i][dy+j] == 0:
                            count += 1
                    else:
                        count += 1
    print(count)
    
if __name__ == "__main__":
    main()
```
