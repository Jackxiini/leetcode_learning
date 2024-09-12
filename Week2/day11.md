# 代码随想录 Day11

## 150. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

[Course Link](https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

自己写的直抒胸臆的写法，有一些小繁琐，但比较直观。

```
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for i in tokens:
            if i.isdigit():
                stack.append(int(i))
            else:
                if i == "+":
                    val = stack.pop() + stack.pop()
                    stack.append(int(val))
                elif i == "-":
                    first = stack.pop()
                    second = stack.pop()
                    val = second-first
                    stack.append(int(val))
                elif "-" in i:
                    stack.append(int(i))
                elif i == "*":
                    val = stack.pop() * stack.pop()
                    stack.append(int(val))
                elif i == "/":
                    first = stack.pop()
                    second = stack.pop()
                    val = second/first
                    stack.append(int(val))
        return int(stack[0])
```

改进一下

```
from operator import add, sub, mul

class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        def div(a, b):
            return int(a/b)
        stack = []
        operators = {"+":add, "-":sub, "*":mul, "/":div}

        for i in tokens:
            if i not in operators:
                stack.append(int(i))
            else:
                first = stack.pop()
                second = stack.pop()
                stack.append(operators[i](second, first))
        return stack.pop()
```

## 239. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

[Course Link](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html)

要用单调队列，每次加新数进去，要把它前面比它小的给去掉

```
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        def push(que, x):
            while que and que[-1]<x:
                que.pop()
            que.append(x)
        
        que = deque()
        for i in range(k):
            push(que, nums[i])
        res = [que[0]]
        for i in range(k, len(nums)):
            head = nums[i-k]
            if head == que[0]:
                que.popleft()
            push(que, nums[i])
            res.append(que[0])
        return res
```

## 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

[Course Link](https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html)

要用heapq库来做最小堆，因为要取最大频率的数，所以要把频率取负来打造最小堆

```
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        from collections import Counter
        import heapq
        count = Counter(nums)
        heap = []
        res = []
        for key, value in count.items():
            heapq.heappush(heap, (-value, key))
        for i in range(k):
            res.append(heapq.heappop(heap)[1])
        return res
```
