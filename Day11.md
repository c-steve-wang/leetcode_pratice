# 代码随想录Day10

leetcode 150, 239, 347

## [leetcode 150]((https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

这个题对于我来说没有难度，acm死去的记忆开始攻击我，直接用stack然后每次返回前两个数子进行运算就好了，但是边界条件要注意
除法必须区取整数，其次这个题有个隐藏坑点就是输入的全是字符，但是最后又要求数字，所以所有的必须取整

```Python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        operations = ['+', '-', '*', '/']
        for i in tokens:
            if i in operations:
                temp2 = stack.pop()
                temp1 = stack.pop()
                ans = 0 
                if i == '+':
                    ans = int(temp1) + int(temp2)
                elif i == '-':
                    ans = int(temp1) - int(temp2)
                elif i == '*':
                    ans = int(temp1) * int(temp2)
                elif i == '/':
                    ans = int(int(temp1) / int(temp2))
                stack.append(ans)
            else:
                stack.append(int(i))

        return stack.pop()
```
## [leetcode 239]
这个题还是要二刷
```Python
from collections import deque


class MyQueue: #单调队列（从大到小
    def __init__(self):
        self.queue = deque() #这里需要使用deque实现单调队列，直接使用list会超时
    
    #每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等则弹出。
    #同时pop之前判断队列当前是否为空。
    def pop(self, value):
        if self.queue and value == self.queue[0]:
            self.queue.popleft()#list.pop()时间复杂度为O(n),这里需要使用collections.deque()
            
    #如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止。
    #这样就保持了队列里的数值是单调从大到小的了。
    def push(self, value):
        while self.queue and value > self.queue[-1]:
            self.queue.pop()
        self.queue.append(value)
        
    #查询当前队列里的最大值 直接返回队列前端也就是front就可以了。
    def front(self):
        return self.queue[0]
    
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        que = MyQueue()
        result = []
        for i in range(k): #先将前k的元素放进队列
            que.push(nums[i])
        result.append(que.front()) #result 记录前k的元素的最大值
        for i in range(k, len(nums)):
            que.pop(nums[i - k]) #滑动窗口移除最前面元素
            que.push(nums[i]) #滑动窗口前加入最后面的元素
            result.append(que.front()) #记录对应的最大值
        return result
```

## [leetcode 347]

这个题似乎可以直接用字典做，但是大顶堆和小顶堆的逻辑还是要掌握
```Python
#时间复杂度：O(nlogk)
#空间复杂度：O(n)
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        #要统计元素出现频率
        map_ = {} #nums[i]:对应出现的次数
        for i in range(len(nums)):
            map_[nums[i]] = map_.get(nums[i], 0) + 1
        
        #对频率排序
        #定义一个小顶堆，大小为k
        pri_que = [] #小顶堆
        
        #用固定大小为k的小顶堆，扫描所有频率的数值
        for key, freq in map_.items():
            heapq.heappush(pri_que, (freq, key))
            if len(pri_que) > k: #如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                heapq.heappop(pri_que)
        
        #找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒序来输出到数组
        result = [0] * k
        for i in range(k-1, -1, -1):
            result[i] = heapq.heappop(pri_que)[1]
        return result
```
