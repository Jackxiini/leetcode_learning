# 代码随想录 Day29

## 134. [Gas Station](https://leetcode.com/problems/gas-station/)

[Course Link](https://programmercarl.com/0134.%E5%8A%A0%E6%B2%B9%E7%AB%99.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

我最开始的想法是先获得差值，再找到连续正值的和的最大值的起始点，那个起始点一定就是答案。试了一下是可以AC的。

```
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        diff = [gas[i] - cost[i] for i in range(len(gas))]
        total_sum = sum(diff)
        if total_sum < 0:
            return -1

        max_sum = 0
        start_index = 0
        current_sum = 0
        
        for i in range(len(diff)):
            current_sum += diff[i]
            if current_sum < 0:
                current_sum = 0
                start_index = i + 1
            elif current_sum > max_sum:
                max_sum = current_sum

        return start_index
```

Link 里的说法是在迭代中如果差值的和小于0了，那么之前走过的点一定都不是起始点（因为不管从这里面的哪个点开始走都通不过当前卡壳的地方），从 i+1再试。

一开始不太能理解括号里的内容，但是后来想明白，其实是因为连续的路径上和都是大于0，说明油是不断的，不管从当中哪个点开始走，都会失去这个点之前的那些多出来的油，如果一开始拥有多出来的这点油的起始点都通不过卡壳的点，那么当中这个点也一定通不过。

```
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        start = 0
        cur_sum = 0
        total = 0
        for i in range(len(gas)):
            diff = gas[i]-cost[i]
            total += diff
            cur_sum += diff
            if cur_sum < 0:
                start = i+1
                cur_sum = 0
        
        if total < 0:
            return -1
        return start
```

## 135. [Candy](https://leetcode.com/problems/candy/)

[Course Link](https://programmercarl.com/0135.%E5%88%86%E5%8F%91%E7%B3%96%E6%9E%9C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

一开始想要考虑两边的情况来更新数值，发现确实会出错。应该先考虑比较是不是比左边大，如果是就左边值+1，到底再回头考虑是不是比右边大，如果是就取右边和目前值的最大值（因为这时候当前已经有一个值在了，所以不需要直接右边+1）

```
class Solution:
    def candy(self, ratings: List[int]) -> int:
        res = [1]*len(ratings)
        for i in range(1,len(ratings)):
            if ratings[i] > ratings[i-1]:
                res[i] = res[i-1]+1

        for i in range(len(ratings)-2,-1,-1):
            if ratings[i] > ratings[i+1]:
                res[i] = max(res[i], res[i+1]+1)
        return sum(res)
```

## 860. [Lemonade Change](https://leetcode.com/problems/lemonade-change/)

[Course Link](https://programmercarl.com/0860.%E6%9F%A0%E6%AA%AC%E6%B0%B4%E6%89%BE%E9%9B%B6.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

很简单，把各种情况摊开逐个分析就对了。唯一要注意的是找 20 元的时候优先给 10+5，因为 10 只能给 20 找，而 5 块更加有用，也可以找给 10。

```
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        pocket = {5:0, 10:0}
        for i in range(len(bills)):
            if bills[i] == 5:
                pocket[5] += 1
            if bills[i] == 10:
                if pocket[5]==0:
                    return False
                pocket[5]-=1
                pocket[10]+=1
            if bills[i] == 20:
                if pocket[10]>=1 and pocket[5]>=1:
                    pocket[10]-=1
                    pocket[5]-=1
                elif pocket[5]>=3:
                    pocket[5]-=3
                else:
                    return False
        return True
```
