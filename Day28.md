# 代码随想录Day28

leetcode 122, 55,45,1005

[leetcode 122]

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
[leetcode 55]

这个题主要要注意python不能直接写，要么if 要么while
这个题的思路就是求覆盖范围，只要覆盖范围大于终点就可以，不要纠结


[leetcode 45]

还是覆盖范围，但是要记录下一步最远的
cur_dis 和 next_dis相当于不断延长

[leetcode 1005]

这个题基本想到了，两次贪心，但是sort里面加lambda，然后reverse要注意
还有求奇偶就可以了，不用连续遍历
