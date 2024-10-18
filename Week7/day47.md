# 代码随想录 Day47

## 42. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

[Course Link](https://programmercarl.com/0042.%E6%8E%A5%E9%9B%A8%E6%B0%B4.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

有两种解法，一种用双指针，一种用单调栈。

首先双指针法，通过观察可以发现我们需要找到的是左边和右边比较低的那个，然后按照当前的高度与较低的那个值的差填满水。那么我们可以创建两个数组，一个记录每个位置左边的最高值（或者这么想，你站在每个位置上往左边看，能看到的那个位置的值是多少，看不到说明当前就是最高，那么记录当前的值），另一个记录每个位置右边的最高值（同理），然后就可以知道每个位置应该填多少水了。

```
class Solution:
    def trap(self, height: List[int]) -> int:
        left, right = [0]*len(height), [0]*len(height)
        res = 0
        left[0] = height[0]
        for i in range(1, len(height)):
            left[i] = max(left[i-1], height[i])
        right[-1] = height[-1]
        for i in range(len(height)-2,-1,-1):
            right[i] = max(right[i+1], height[i])
        for i in range(len(height)):
            res+=(min(left[i], right[i])-height[i])
        return res
```

单调栈，试了一下比双指针还快。代码是精简过的，实际上有三种情况。第一种是 height[i] < height[stack[-1]]，很显然因为我们在建立单调递增的栈，所以肯定是把当前值 push 进去的。另一种是 两者相等，那么我们先 pop 掉当前值再 push 一个 i 进去（这里为什么要这么操作，后面就会明白，因为我们要利用单调栈来找到现在这个凹点的左边墙和右边墙，所以我们不希望里面有一样高度的值）。最后就是 height[i] > height[stack[-1]]，我们需要在这种情况下获取左边墙和右边墙的高度，右边就是 height[i]，左边就是 pop 完后的 height[stack[-1]]（这里就能知道为什么不想留一样的高度进单调栈了），然后我们就可以获得水高，然后利用下标，能知道当前的宽度是多少，就能知道这一层的水是多少格了。（这里值得注意的是，我们仅需要求那一行的水，而不需要提前求出来这一列是多少，打个比方，32123，我们只需要求出来 212 这里，2 到 2 这一行有一格水，不需要求出来 1上面，3那个高度还有一格，因为遍历到 3 的时候，我们会求出来 3到 3 那一行有多少水的。）

精简的代码，第一种情况很显然可以忽略，因为最后一定会 push i 进去的。第二种情况为什么可以省略，因为当我们单调栈里有相同值的时候，算 h 会算出来 0，res不会加任何值，相当于白算一次，等同于给它 pop 掉了。

```
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = []
        res = 0
        for i in range(len(height)):
            while stack and height[i] > height[stack[-1]]:
                mid = stack.pop()
                if stack: 
                    h = min(height[i], height[stack[-1]])-height[mid] # min(right, left)
                    w = i-stack[-1]-1
                    res+=h*w
            stack.append(i)
        return res
```
