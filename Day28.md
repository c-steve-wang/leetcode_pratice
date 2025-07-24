# 代码随想录Day28

leetcode 55,45,1005

[leetcode 55]

这个题之前做过，直接找每次的递增就好了

```Python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit = 0
        for i in range(1, len(prices)):
            if prices[i] > prices[i - 1]:
                max_profit += prices[i] - prices[i-1]
            

        return max_profit
```
