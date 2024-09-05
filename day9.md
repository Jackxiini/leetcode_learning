# 代码随想录 Day9

## 151. [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

[Course Link](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

Python中求解这题很简单，而且Python中 string 是不可变的类型因此好像无法使空间复杂度降为O(1)。

```
class Solution:
    def reverseWords(self, s: str) -> str:
        word = s.split()
        word = word[::-1]
        return ' '.join(word)
```

```
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.strip()
        s = s[::-1]
        return ' '.join([word[::-1] for word in s.split()])
```

## [右旋字符串](https://kamacoder.com/problempage.php?pid=1065)

[Course Link](https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)

```
k = int(input())
s = input()

def rotate_string(k, s):
    len_s = len(s)
    move = k % len_s
    return s[-move:] + s[:len_s-move]

print(rotate_string(k, s))
```
