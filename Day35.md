# 代码随想录Day35

前面的零一背包问题还是比较好理解的，走通了这五步其实问题不大，最重要的是如果降低空间复杂度的话，遍历顺序必须从后向前，不然前面的会被用两次，就变成完全背包问题了

[leetcode 416](https://leetcode.com/problems/partition-equal-subset-sum)

这个地方写的时候还是有不少问题，尤其出现在如何递归上， 同时要注意除法双斜杠才是是整数，最后如果用二维背包写这个题，那么我们应该怎么定义行列，怎么构建二维数组

```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums) % 2 != 0:
            return False
        target_sum = sum(nums) // 2
        dp = [0]*(target_sum + 1)

        for i in range(len(nums)):
            for j in range(target_sum, nums[i]-1, -1):
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])

        return dp[-1] == target_sum 
        

```



