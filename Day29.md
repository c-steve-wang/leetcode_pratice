# 代码随想录Day29

leetcode 134,135,860, 406

[leetcode 134](https://leetcode.com/problems/gas-station/)

这个地方因为有total的存在，如果start没更新，那么start之后必然> 0， 这个时候total也大于零，则之前的必然小于start到结尾，则可以跑完一圈，因为这个思想所以可以一次循环结束绕圈
```Python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        total_sum = 0 
        cur_sum = 0 
        start = 0 

        for i in range(0, len(gas)):
            cur_sum += gas[i] - cost[i]
            total_sum += gas[i] - cost[i]
            if cur_sum < 0:
                start = i + 1
                cur_sum = 0
            
        if total_sum < 0:
            return -1
        else:
            return start
```

[leetcode 135](https://leetcode.com/problems/candy/)
这个地方注意两点，一个是从左到右比一次，直接加一就可以，另外一个是从右到左必须取max，比如左边是5，本身是4，但是右边是2，这种情况下只考虑一个方向不能完全比较

```
class Solution:
    def candy(self, ratings: List[int]) -> int:
        candy = [1]*len(ratings)
        for i in range(1, len(ratings)):
            if (ratings[i] > ratings[i-1]):
                candy[i] = candy[i - 1] + 1
        
        for i in range(len(ratings) - 2, -1, -1):
            if (ratings[i] > ratings[i + 1]):
                candy[i] = max(candy[i+1] + 1, candy[i])

        return sum(candy)
```

[leetcode 860](https://leetcode.com/problems/lemonade-change/description/)

这个地方一开始想错了，是纸币个数不是单纯的数值问题

```
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five= 0 ;ten = 0 ; twenty = 0
        for i in bills:
            if i == 5:
                five += 1
            elif i == 10:
                if five <= 0:
                    return False
                else:
                    five -= 1
                    ten += 1
            elif i == 20:
                if ten > 0 and five > 0:
                    ten -= 1
                    five -= 1
                    twenty += 1
                elif five >= 3:
                    five -= 3
                else:
                    return False

        return True
```
[leetcode 406](https://leetcode.com/problems/lemonade-change/description/)

这个地方其实主要是思路
但是对于我来说，一个是插入函数怎么操作，另外一个是怎么同时排序

```
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x: (-x[0], x[1]))
        que = []

        for p in people:
            que.insert(p[1], p)
        return que
```
