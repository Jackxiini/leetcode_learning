# 代码随想录 Day8

## 344. [Reverse String](https://leetcode.com/problems/reverse-string/)

[Course Link](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)

用 Python 的奇技淫巧可以有很多一行解，例如 s[:]=s[::-1]，但是这样练习就没有意义了，因此这里用了双指针，更多解法看链接

```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right=0, len(s)-1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left+=1
            right-=1
```

## 541. [Reverse String II](https://leetcode.com/problems/reverse-string-ii/)

[Course Link](https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html)

```
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        count = 0
        for i in range(0,len(s),k):
            if count%2==0:
                s = s[:count*k]+s[count*k:(count*k+k)][::-1]+s[(count*k+k):]
            count+=1
        return s
```
