# 代码随想录Day37&38

leetcode 518,377

[leetcode 518](https://leetcode.com/problems/coin-change-ii/)

这个地方遍历犯了两个问题，我们这个用的是一维滚动数组，我们最少的容量都是coins\[i\] 所以要确保j-coins\[i == 0\]起码要大于零
这个是完全背包里面要注意的问题，然后算价值和算方法数的递推公式是不一样的
其他的都是完全背包的样子了

```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:

        # method to make up, the dp is dp[j] += dp[j - coins[i]]
        dp = [0]*(amount + 1)

        dp[0] = 1

        for i in range(len(coins)):
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j - coins[i]]

        return dp[amount]


```


[leetcode 377](https://leetcode.com/problems/combination-sum-iv/)

这个地方虽然题目写了combination，但是整体上求的是排列数，所以问题出在这个地方，其他的都和上一题一模一样

```
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)

        dp[0] = 1

        for j in range(target + 1):
            for i in range(len(nums)):
                if j - nums[i] >= 0:
                    dp[j] += dp[j - nums[i]]

        return dp[target]
```


[leetcode 322](https://leetcode.com/problems/coin-change/)

这个题只要想明白了递推公式没什么难度， 对于我主要难在edge case的判断，这个完全在写的时候没想到
如果最后依然最大的话，说明没有更新，则说明没有最小情况，应该返回 -1

```
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:

        dp =[float('inf')] * (amount + 1)
        dp[0] = 0
        for i in range(len(coins)):
            for j in range(coins[i], amount + 1):
                dp[j] = min(dp[j], dp[j - coins[i]] + 1)

        if dp[amount] == float('inf'):
            return -1
        return dp[amount]
        
```


[leetcode 279](https://leetcode.com/problems/combination-sum-iv/)

这个题被边界条件难到了，主要还是要从1开始，而且如果按照我这种list写也是一样的
最好还是按照题解里面直接把平方加到循环里面

```
class Solution:
    def numSquares(self, n: int) -> int:
        sq_list = []
        i = 1
        while i * i <= n:
            sq_list.append(i * i)
            i += 1
        
        len_sq = len(sq_list)
        
        dp = [float('inf')] * (n+1)
        dp[0] = 0

        for j in range(1, n+1):
            for i in range(len_sq):
                if sq_list[i] <= j:
                    dp[j] = min(dp[j], dp[j - sq_list[i]] + 1)

        return dp[n]
```


[leetcode 139](https://leetcode.com/problems/combination-sum-iv/)

这个地方出问题的点在于判断条件和dp数组的含义

dp\[i\] 表示这个长度能拆了出来一个或多个单词字串，相当于从字串往下遍历


```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        wordSet = set(wordDict)

        n = len(s)

        dp = [False] * (len(s) + 1)
        dp[0] = True

        # keep the order, bag first

        for j in range(1, n + 1):
            for i in range(j):
                if dp[i] == True and s[i:j] in wordSet:
                    dp[j] = True
                    break

        return dp[n]
```
