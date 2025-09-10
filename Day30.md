# 代码随想录Day30

leetcode 452, 435, 763

[leetcode 452](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
这个地方理解了思路还是很容易的，主要是重叠区间下贪心，通过更新区间右边界来保证重叠，同时因为最少需要一个箭，所以起始是1

```
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        result = 1
        points.sort(key = lambda x: x[0])
        #print(points)
        for i in range(1, len(points)):
            if points[i][0] > points[i-1][1]:
                result += 1
            else:
                points[i][1] = min(points[i-1][1], points[i][1])

        return result
```

[leetcode 435](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
和上一题思路是一样的，这个地方想复杂了，而且code优化可以直接if，不需要continue

```
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        result = 0
        intervals.sort(key = lambda x: x[0])
        print(intervals)
        for i in range(1, len(intervals)):
            if intervals[i][0] >= intervals[i - 1][1]:
                continue
            else:
                intervals[i][1] = min(intervals[i-1][1], intervals[i][1])
                result += 1

        return result 

```
[leetcode 763](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

这个题最重要的也是右边界，但是这个右边界用哈希表记录位置就可以，然后不断更新
```
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        hash = ['-inf'] * 27

        for i in range(len(s)):
            hash[ord(s[i]) - ord('a')] = i

        left = 0; right = 0 
        result = []
        
        for i in range(len(s)):
            right = max(right, hash[ord(s[i]) - ord('a')])
            if i == right:
                lenth = right - left + 1
                result.append(lenth)
                left = right + 1

        return result
```
这个题最重要的也是右边界，但是这个右边界用哈希表记录位置就可以，然后不断更新
