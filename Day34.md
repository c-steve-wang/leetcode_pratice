# 代码随想录Day32

leetcode 62，63， 343， 96

[Leetcode 62](https://leetcode.com/problems/unique-paths/)
这个地方很容易想，但是循环的边界条件还是搞错了，究竟从哪里开始，有没有初始化一定要分清
```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            dp[i][0] = 1
        
        for j in range(n):
            dp[0][j] = 1

        for i in range(1, m):
            for j in range (1, n):
                dp[i][j] = dp[i-1][j] + dp[i][j - 1]


        return dp[m-1][n-1]
```    
[leetcode 63]
这个题这个地方想复杂了，其实碰见了直接continue就好，或者跟现在这个一样，只有为零的时候才加，不需要犹豫，重点是初始化，obstacle可能在哪
```
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(m):
            if obstacleGrid[i][0] == 0:
                dp[i][0] = 1
            else:
                break
        
        for j in range(n):
            if obstacleGrid[0][j] == 0:
                dp[0][j] = 1
            else:
                break

        for i in range(1, m):
            for j in range (1, n):
                if obstacleGrid[i][j] == 0:
                    dp[i][j] = dp[i-1][j] + dp[i][j - 1]
            

        return dp[m-1][n-1]
```
