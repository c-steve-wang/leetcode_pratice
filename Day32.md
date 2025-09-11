# 代码随想录Day32

leetcode 509, 70, 746

[leetcode 509](https://leetcode.com/problems/fibonacci-number/description/)
这个题理论上是简单的，但是犯了两个错误，一个是五步里面的dp数组初始化没有看题出了问题，第二个是确定遍历顺序，另外最后没有打印dp数组查问题，这个题还是很有代表性的

```
class Solution:
    def fib(self, n: int) -> int:

        if n == 0 :
             return n
        dp = [0]*(n+1)
        dp[0] = 0
        dp[1] = 1
        for i in range(2, n+1):
            dp[i] = dp[i-1] + dp[i-2]   

        return dp[n]
```

[leetcode 70](https://leetcode.com/problems/fibonacci-number/description/)
突然开悟，这个地方的写法虽然跟上一个类似，但是含义完全不一样，就是剩下两种直接一步到位的方法，然后就是之前的路径方法 + 1或者2

```
def climbStairs(self, n: int) -> int:
        if n <= 1:
            return n
        
        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 2
        
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        
        return dp[n]
```
[leetcode 746](https://leetcode.com/problems/fibonacci-number/description/)

这个地方主要是推公式
注意两点 1. dp[0]和dp[1]都应该设置成0， 因为能要么从1开始要么从0 开始，其次，跳了才算cost，不然不算cost
其实最重要的还是状态转移，如何转移状态，这个是楼梯问题里面最关键的
```
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        if len(cost) <=1:
            return 0
        
        n = len(cost)
        dp = [0]*(n+1)
        dp[0] = 0
        dp[1] = 0 

        for i in range(2, n+1):
            dp[i] = min(dp[i-1] + cost[i - 1], dp[i-2] + cost[i - 2])

        
        return dp[n]
```
