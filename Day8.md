# 代码随想录Day7

leetcode 344,541

## [leetcode 344](https://leetcode.com/problems/reverse-string/)

这个地方比较简单，我比较喜欢双指针和栈，reverse作为一种补充可以用

```Python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        while left<right:
            s[left],s[right] = s[right], s[left]
            left += 1
            right -= 1
```

```Python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        stack = []

        for char in s:
            stack.append(char)
        for i in range(len(s)):
            s[i] = stack.pop()
```

## [leetcode 541](https://leetcode.com/problems/reverse-string-ii/)
这个也比较简单，直接ac,思路应该是一致的，但我这里用的python属性比较多,我觉得那个直接切片换的是最好的
```Python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        result = ""
        for i in range(0,len(s), 2*k):
            
            temp1 = list(s[i:i+k])
            temp2 = s[i+k:i+2*k]
            temp1.reverse()
            result = result + ''.join(temp1) +temp2
        return result
```

我认为更好的：
```Python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        i = 0
        chars = list(s)
        
        while i < len(chars):
            chars[i:i + k] = chars[i:i + k][::-1] # 反转后，更改原值为反转后值
            i += k * 2

        return ''.join(chars)
```

## 替换数字

这个地方我觉得比较简单，算是之前的拓展
