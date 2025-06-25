# 代码随想录Day1

leetcode 704, 27, 977

[leetcode 704](https://leetcode.com/problems/binary-search/description/)

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
