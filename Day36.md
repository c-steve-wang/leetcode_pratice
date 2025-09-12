# 代码随想录Day36

leetcode 1049,494, 474

[leetcode 416](https://leetcode.com/problems/partition-equal-subset-sum)

这道题是我的问题，忘记了01背包的含义，尤其是dp，即在容量为j的情况下的最大价值，这样的话就能很好的换用成dp问题，就很方便了

```
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        target = sum(stones) // 2
        
        dp = [0]*(target + 1)

        for i in range(len(stones)):
            for j in range(target, stones[i] - 1, -1):
                dp[j] = max(dp[j], dp[j-stones[i]] + stones[i])


        return sum(stones) - dp[target] - dp[target]
        

```
[leetcode 494](https://leetcode.com/problems/partition-equal-subset-sum)
这个地方要留意几点，第一个是如果left**不能被整除**则说明没有答案，比如case 1里面变成target 3 ，sum4的情况，这个是因为left必然是整数
其次要注意dp[0]必须是1，因为是1才能往下dp，第二个是不取也是一种方法，
这个地方是所有求方法数的01背包问题的递推公式
```
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        if abs(target) > sum(nums):
            return 0

        if (target + sum(nums)) % 2 == 1:
            return 0

        target_sum = (target + sum(nums)) // 2

        dp = [0]*(target_sum + 1)
        dp[0] = 1

        for i in range(len(nums)):
            for j in range(target_sum, nums[i] - 1, -1):
                dp[j] += dp[j - nums[i]]

        
        return dp[target_sum]
```
