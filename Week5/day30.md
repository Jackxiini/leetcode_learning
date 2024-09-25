# 代码随想录 Day30

三道重叠问题

## 452. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

[Course Link](https://programmercarl.com/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.html#%E6%80%9D%E8%B7%AF)

很容易想到将初始位置（或者末尾）排序，然后如果下一个起球的初始在末尾前，那么就是重叠，就只需要一支箭。但是仅这样是不对的，可以尝试一下 [[0,9],[0,6],[7,12]]，答案应该是2，但是如果只是用之前的规律会算出来1。还需要在迭代的时候更新末尾为最小值。

```
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        points.sort(key=lambda x:x[0])
        start, end= points[0][0], points[0][1]
        arrow = 1
        for i in range(len(points)):
            if points[i][0] > end:
                arrow += 1
                start = points[i][0]
                end = points[i][1]
            end = min(end, points[i][1])
        return arrow
```

## 435. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

[Course Link](https://programmercarl.com/0435.%E6%97%A0%E9%87%8D%E5%8F%A0%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

和上一题很像，也可以左或右排序，如果当前的头比前一个尾小那么重合，res + 1并且末尾改成最小末尾。

先自己写了一遍右排序的，感觉可以简化

```
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x:x[1])
        start = intervals[0][0]
        end = intervals[0][1]
        res = 0
        for i in range(1, len(intervals)):
            if intervals[i][0] < end:
                res += 1
            else:
                start = intervals[i][0]
                end = intervals[i][1]
        return res
```

看 Link 写了一遍左排序的

```
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda x:x[0])
        res = 0
        for i in range(1, len(intervals)):
            if intervals[i][0] < intervals[i-1][1]:
                res += 1
                intervals[i][1] = min(intervals[i-1][1], intervals[i][1])
        return res
```

## 763. [Partition Labels](https://leetcode.com/problems/partition-labels/)

[Course Link](https://programmercarl.com/0763.%E5%88%92%E5%88%86%E5%AD%97%E6%AF%8D%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

看了Hint有了思路，先获取 last 字典（记录每一个出现过的字母最后出现的 index），然后遍历，当遍历的 index 达到其中字母出现过的最大index（end），则记录结果。例如 'ababc'，从 a 遍历，到 b 的时候需要更新 end，因为 b 比 a 更晚出现过，直到 i 到了 end，记录结果，继续往下遍历。

```
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        last = {}
        res = []
        start = 0
        end = 0
        for i in range(len(s)):
            last[s[i]] = i
        for i in range(len(s)):
            end = max(last[s[i]], end)
            if i == end:
                res.append(end-start+1)
                start = end+1
        return res
```
