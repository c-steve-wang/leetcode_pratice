# 代码随想录Day27

leetcode 455，376，53

[leetcode 455]

主要是胃口和饼干的嵌套顺序不能搞混，感觉这个是简单极端，另外内层遍历用index控制

```Python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        res = 0 
        index = len(s) - 1
        for i in range(len(g)-1, -1, -1):
            if index >= 0 and s[index] >= g[i]:
                res += 1
                index -=1
        return res
```


[leetcode 538]
这个题主要要注意三种情况，就先不写了

[leetcode 108]
这个地方不是遇到负数跳过，而是连续和小于零没有正向增大作用就跳过
