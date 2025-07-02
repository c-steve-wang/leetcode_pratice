# 代码随想录Day8

leetcode 151， 28, 459

## [leetcode 151](https://leetcode.com/problems/reverse-string/)

这个题对于做nlp的比较好理解也容易做，一遍ac，主要还是python的，除了我现在的方法，我第一时间能想到的还有栈和左右指针
```Python
class Solution:
    def reverseWords(self, s: str) -> str:
        temp = s.split()
        temp.reverse()
        result = " ".join(temp)
        return result
        # for i in range(0, len(temp) - 1):
        #     result += temp[i] + " "
        
        # result += temp[len(temp) - 1]
        # return result

```
## [leetcode 28](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

kmp算法目前算是大致理解了，其实主要还是中心思想：“充分利用已经有的公共序列”
但是现在的解法应该还是偏死记硬背，之后二刷的时候要再看一遍

```Python
class Solution:
    def getNext(self, next, s: str):
        j = -1 
        next[0] = j
        for i in range(1, len(s)):
            while j>=0 and s[i]!=s[j+1]:
                j = next[j]
            if s[i] == s[j+1]:
                j += 1
            next[i] = j

    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0 
        next = [0]*len(needle)
        self.getNext(next, needle)
        j = -1
        for i in range(len(haystack)):
            while j >= 0 and haystack[i] != needle[j+1]:
                j = next[j]
            if haystack[i] == needle[j+1]:
                j += 1
            if j == len(needle) - 1:
                return i - len(needle) + 1
        return -1
```

## [leectcode 459](https://leetcode.com/problems/repeated-substring-pattern/)

这个地方难的是证明为什么最长相等前后缀的之和的剩下的部分是最小元素，理解这个思路
另外移动匹配也是一个比较好的办法， 最后python里面可以用find这些函数，所以kmp算法本身理解基本思想就行，没有必要死磕在这

```Python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        if n <= 1:
            return False
        ss = s[1:] + s[:-1] 
        print(ss.find(s))              
        return ss.find(s) != -1
```
