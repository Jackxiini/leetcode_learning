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
