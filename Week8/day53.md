# 代码随想录 Day53

### [并查集理论基础](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E5%B9%B6%E6%9F%A5%E9%9B%86%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E8%83%8C%E6%99%AF)

```
# 初始化参数
n = 1005  # n的值可以根据具体问题调整
father = list(range(n))  # 初始化并查集的父节点数组

# 并查集初始化
def init():
    global father
    father = list(range(n))

# 并查集里寻根的过程，带路径压缩
def find(u):
    if u == father[u]:
        return u
    else:
        father[u] = find(father[u])  # 路径压缩
        return father[u]

# 判断 u 和 v 是否找到同一个根
def is_same(u, v):
    return find(u) == find(v)

# 将 v -> u 这条边加入并查集
def join(u, v):
    root_u = find(u)  # 寻找 u 的根
    root_v = find(v)  # 寻找 v 的根
    if root_u != root_v:  # 如果根不同，则将 v 的根指向 u 的根
        father[root_v] = root_u
```

## 107. [寻找存在的路径](https://kamacoder.com/problempage.php?pid=1179)

[Course Link](https://www.programmercarl.com/kamacoder/0107.%E5%AF%BB%E6%89%BE%E5%AD%98%E5%9C%A8%E7%9A%84%E8%B7%AF%E5%BE%84.html#%E6%80%9D%E8%B7%AF)

纯模板题，判断是否存在路径，就是判断这两个点的根是否一样。

```
class UnionFind:
    def __init__(self, n):
        self.father = list(range(n+1))
    
    # 并查集里寻根的过程，带路径压缩
    def find(self, u):
        if u == self.father[u]:
            return u
        else:
            self.father[u] = self.find(self.father[u])  # 路径压缩
            return self.father[u]
    
    # 判断 u 和 v 是否找到同一个根
    def is_same(self, u, v):
        return self.find(u) == self.find(v)
    
    # 将 v -> u 这条边加入并查集
    def join(self, u, v):
        root_u = self.find(u)  # 寻找 u 的根
        root_v = self.find(v)  # 寻找 v 的根
        if root_u != root_v:  # 如果根不同，则将 v 的根指向 u 的根
            self.father[root_v] = root_u


def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    index = 0
    n = int(data[index])
    index += 1
    m = int(data[index])
    index += 1
    
    uf = UnionFind(n)
    
    for _ in range(m):
        s = int(data[index])
        index += 1
        t = int(data[index])
        index += 1
        uf.join(s, t)
    
    source = int(data[index])
    index += 1
    destination = int(data[index])
    
    if uf.is_same(source, destination):
        print(1)
    else:
        print(0)
    
if __name__ == "__main__":
    main()
```
