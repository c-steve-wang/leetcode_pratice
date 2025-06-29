# 代码随想录Day2

leetcode 209,59


## [leetcode 209](https://leetcode.com/problems/minimum-size-subarray-sum/)

这里面要记住两点
1.滑动窗口主循环是终止位置，因为如果是起始位置就会回退到暴力的解法
2.滑动窗口的起始位置移动要用while 而不是if因为要检查每一个

最后有一个细节要注意就是闭区间长度的计算是 end - start + 1

```Python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        cur = 0
        sum = 0
        minL = 1e6
        n = len(nums)
        for i in range(n):
            sum += nums[i]
            while sum >= target:
                tempL = i - cur + 1
                minL = min(tempL, minL)
                sum = sum - nums[cur]
                cur += 1
        if minL == 1e6:
            return 0
        return minL
```

## [leetcode 59](https://leetcode.com/problems/minimum-size-subarray-sum/)
这个地方用python的话可以循环offset，不需要讲解里面那么去循环，剩下的其实也想到了，最重要的还是左闭右开的条件
```Python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0]*n for _ in range(n)]
        loop, mid = n//2, n//2
        count = 1
        startx, starty = 0 ,0

        for offset in range(1, loop + 1):
            for i in range(starty, n - offset) :    # 从左至右，左闭右开
                nums[startx][i] = count
                count += 1
            for i in range(startx, n - offset) :    # 从上至下
                nums[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左
                nums[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上
                nums[i][starty] = count
                count += 1                
            startx += 1         # 更新起始点
            starty += 1
        if n % 2 != 0 :			# n为奇数时，填充中心点
            nums[mid][mid] = count 
        return nums

```

剩下两个主要理解思路，其他的没必要赘述

