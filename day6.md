# 代码随想录 Day6

#### 当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法。

## 242. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)

[Course Link](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)

```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import Counter
        s_count = Counter(list(s))
        t_count = Counter(list(t))
        return s_count == t_count
```

## 349. [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

[Course Link](https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html)
