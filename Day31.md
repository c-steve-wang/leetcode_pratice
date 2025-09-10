# 代码随想录Day31

leetcode 56,738,968

[leetcode 56](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
总算改对了，这个地方忽略了连续的情况，光想着套模板了，实际上就是不断维护和比较右边界

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        result = []

        intervals.sort(key = lambda x: x[0])
        left = intervals[0][0]
        right = intervals[0][1]

        for i in range (1, len(intervals)):

            if right >= intervals[i][0]:
                right = max(right, intervals[i][1])
            else:
                result.append([left, right])
                left = intervals[i][0]
                right = intervals[i][1]

        result.append([left,right])
        
        return result
        
```


[leetcode 738](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
这个地方其实就是十进制的思路，从后往前两位两位检查，然后就是前一位减一，后面都是9
```
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        str_num = list(str(n))

        for i in range(len(str_num ) -1 ,0, -1):
            if str_num[i - 1] > str_num[i]:
                str_num[i - 1] = str(int(str_num[i-1]) - 1)
                for j in range(i, len(str_num)):
                    str_num[j] ='9'

        return int(''.join(str_num))
```


[leetcode 968](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
