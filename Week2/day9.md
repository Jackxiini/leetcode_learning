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

## 28. [Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

[Course Link](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

重要算法 KMP，得多看几遍，比较难
```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def getnext(needle):
            next_list = [0] * len(needle)
            j = 0
            for i in range(1, len(needle)):
                while j > 0 and needle[i]!=needle[j]:
                    j = next_list[j-1]
                if needle[i]==needle[j]:
                    j+=1
                next_list[i]=j
            return next_list
        
        next_list = getnext(needle)
        j = 0
        for i in range(len(haystack)):
            while j>0 and haystack[i]!=needle[j]:
                j = next_list[j-1]
            if haystack[i] == needle[j]:
                j+=1
            if j == len(needle):
                return i-len(needle)+1
        return -1
```

## 459. [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/description/)

[Course Link](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

感觉也不容易，主要是应用KMP，然后找到规律，字符串长度减去 next（tmp）数组的最后一个值就是单个子字符串的长度，这个长度需要能被 tmp 最后一个值整除，并且注意 tmp 最后一个值不能是0，否则相当于字符串内没有重复。

```
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        def getnext(s):
            tmp = [0] * len(s)
            j = 0
            for i in range(1, len(s)):
                while j>0 and s[i]!=s[j]:
                    j = tmp[j-1]
                if s[i]==s[j]:
                    j+=1
                tmp[i]=j
            return tmp
        
        tmp = getnext(s)
        if tmp[-1]!=0 and tmp[-1]%(len(s)-tmp[-1])==0:
            return True
        return False
```
