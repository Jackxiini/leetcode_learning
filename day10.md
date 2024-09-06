# 代码随想录 Day10

## 232. [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

[Course Link](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

```
class MyQueue:

    def __init__(self):
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        self.stack_in.append(x)

    def pop(self) -> int:
        if not self.stack_out:
            while self.stack_in:
                self.stack_out.append(self.stack_in.pop())
        return self.stack_out.pop()

    def peek(self) -> int:
        if self.stack_out:
            return self.stack_out[-1]
        else:
            return self.stack_in[0]

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

## 225. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

[Course Link](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

Python里得用 deque 来模拟队列，top里不要用 self.que_in[-1] 来获取 top 值因为这还是用到了栈的方法，可以复用 pop里的代码，再把pop出去的append回去。pop里的代码可以用 que_in, que_out 对换的方式来简化（不用把数挪来挪去）。

```
from collections import deque

class MyStack:

    def __init__(self):
        self.que_in = deque()
        self.que_out = deque()

    def push(self, x: int) -> None:
        self.que_in.append(x)

    def pop(self) -> int:
        if self.empty():
            return None

        for _ in range(len(self.que_in)-1):
            self.que_out.append(self.que_in.popleft())
        self.que_in, self.que_out = self.que_out, self.que_in

        return self.que_out.popleft()

    def top(self) -> int:
        if self.empty():
            return None

        for _ in range(len(self.que_in)-1):
            self.que_out.append(self.que_in.popleft())
        self.que_in, self.que_out = self.que_out, self.que_in
        tmp = self.que_out.popleft()
        self.que_in.append(tmp)
        
        return tmp

    def empty(self) -> bool:
        return not (self.que_in or self.que_out)


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

## 20. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

[Course Link](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

栈的经典应用

```
class Solution:
    def isValid(self, s: str) -> bool:
        book = {'(':')','[':']','{':'}'}
        stack = []
        for i in s:
            if i in book.keys():
                stack.append(book[i])
            else:
                if stack and i == stack[-1]:
                    stack.pop()
                else:
                    return False
                    
        return True if not stack else False
```

## 1047. [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

[Course Link](https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

另一题栈的经典应用

```
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for i in s:
            if stack and i == stack[-1]:
                stack.pop()
            else:
                stack.append(i)
        return ''.join(stack)
```
