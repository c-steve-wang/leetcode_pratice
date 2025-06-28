# 代码随想录Day1

leetcode 704, 27, 977

## [leetcode 704](https://leetcode.com/problems/binary-search/description/)

这里首先犯的第一个问题是确定中值，在这个题目中直接 (left + right) // 2 是没什么问题的，但是考虑到越界问题最安全的写法肯定还是 left + （right - left）// 2

其次第二个问题， 我选在用左开右闭，这个区间个人比较好理解，记住left <= right, mid = right - 1, 以及mid = left + 1

最后还要留意更新区间的定义
1. 如果mid > traget ，说明在右边，更新左边界
2. 如果mid < target , 说明在左边，更新右边界

最后是我自己version的ac代码

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l = 0
        r = len(nums) - 1

        while l <= r:
            mid = l + (r - l) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                r = mid - 1
            elif nums[mid] < target:
                l = mid + 1
        return -1

```

## [leetcode 27](https://leetcode.com/problems/remove-element/)

这个题最近做过，但是没有一遍做出来

具体的问题在于判断的时候没有想清楚：因为只考虑了 ！= val的情况，没有考虑 == val的情况，已经跳过去了，所以不等的时候先放，然后p再递增，就已经指向了下一位，主要没考虑清楚nums[i] = nums[j] 这种赋值可以出现在这里

最后放上我debug之后的code

```Python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        p = 0
        for i in range(0,len(nums)):
            if nums[i] != val:
                nums[p] = nums[i]
                p += 1
        
        return p

```

## [leetcode 977](https://leetcode.com/problems/remove-element/)


暴力的的做法

```Python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range(0,len(nums)):
            nums[i] = nums[i]**2

        nums.sort()

        return nums
        
```

这个地方也应该立刻做出来的，毕竟双指针也是第二遍过了，但是这里面看hint的时候陷入了一个误区就是空间复杂度要低，以为是在同一个数组下操作，所以导致这个地方出现了问题。而且这里没考虑到的一点就是最大的是必然在两边的，忽略了很多条件.

还有一点视频里面讲的是做平方计算，但我更喜欢题解里面的求绝对值，减少一步平方运算

根据题解写出来的ac code：

```Python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        result = [0]*len(nums)
        left = 0 
        right = len(nums) - 1
        for i in range(len(nums) -1 , -1, -1):
            if abs(nums[left]) <=  abs(nums[right]):
                square = nums[right]
                right -= 1
            else:
                square = nums[left]
                left += 1
            result[i] = square*square
        return result

```

