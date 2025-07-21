# 代码随想录Day25

leetcode 491,46,47

[leetcode 491](https://leetcode.com/problems/non-decreasing-subsequences/)

这个地方写的时候还是出了不少细节问题，首先要排除空集，跟之前两个题不同，这个地方不包含空集
其次要先判断数组不为空才能取[-1]，这个地方有点顺序要求
最后set要用add（）
整体上想到了思路，想到了去重，但是没想到数层去重，但是听了之后立刻想到用set了

```Python
class Solution:
    def bt(self, nums, start_index, path, res):
        if len(path) > 1:
            res.append(path[:])
        u_set = set()
        for i in range(start_index, len(nums)):
            if nums[i] in u_set or (path and path[-1] > nums[i]):
                continue
            u_set.add(nums[i])
            path.append(nums[i])
            self.bt(nums, i+1, path, res)
            path.pop()

    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        path =[]
        res = []
        self.bt(nums, 0, path, res)
        return res
```

[leetcode 46](https://leetcode.com/problems/non-decreasing-subsequences/)

留意一下全排列问题，不需要向后递归，所以不需要start_index
used数组本身都是一样的，只是记录数据用过了没，这个地方换不换set没必要，这个地方和之前的不一样，需要整体去重，而不是数层去重，所以要不断传入set/used数组，本质上是一样的
used数组本身还简单一点

```Python
class Solution:
    def bt(self,nums, used, res, path):
        if len(path) == len(nums):
            res.append(path[:])
            return
        for i in range(0,len(nums)):
            if used[i]:
                continue
            used[i] = True
            path.append(nums[i])
            self.bt(nums, used, res, path)
            path.pop()
            used[i] = False

    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []
        used = [False]*len(nums)
        self.bt(nums, used, res, path)
        return res
```

[leetcode 49](https://leetcode.com/problems/non-decreasing-subsequences/)

想了一下之前的题直接稍加改动ac了,这个地方要注意 i > 0 这个条件毕竟used[-1]可以是最后一个元素，会导致只有两个的情况下wrong answer

```Python
class Solution:
    def bt(self,nums, used, res, path):
        if len(path) == len(nums):
            res.append(path[:])
            return

        for i in range(0,len(nums)):
            if i > 0 and not used[i - 1] and nums[i] == nums[i-1]:
                continue
            if used[i]:
                continue
            used[i] = True
            path.append(nums[i])
            self.bt(nums, used, res, path)
            path.pop()
            used[i] = False

    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []
        used = [False]*len(nums)
        nums.sort()
        self.bt(nums, used, res, path)
        return res
```
