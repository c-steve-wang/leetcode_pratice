# 代码随想录Day24

leetcode 93,78,90

[leetcode 93](https://leetcode.com/problems/restore-ip-addresses/)

这个题写的时候还是出了很多问题的，比如在什么地方改变成字符串，然后每一个具体的条件是什么，最重要的是怎么分割，一定要记得分割都是从start_index开始然后递归函数是start_index往后，即i+1
```Python
class Solution:
    def is_valid(self,s, start, end):
        if start > end:
            return False
        if s[start] == '0' and start != end:
            return False
        s1 = s[start: end+1]
        for i in s1:
            if not i.isdigit():
                return False
        num = int(s1)
        if num <= 255 and num>=0:
            return True
        else:
            False
    
    def bt(self, s , start_index, path, res):
        if start_index == len(s) and len(path) ==4:
            temp = ".".join(path)
            res.append(temp)
            return
        
        if len(path) > 4:
            return

        for i in range(start_index, len(s)):
            if self.is_valid(s, start_index, i):
                path.append(s[start_index: i+1])
                self.bt(s, i+1, path, res)
                path.pop()


    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []
        path = []
        self.bt(s,0,path,res)
        return res
```
[leetcode 78](https://leetcode.com/problems/restore-ip-addresses/)

这个地方第一点想复杂了，第二点path[:]又漏了

```Python
class Solution:
    def bt(self, nums, start_index, path, res):
        if start_index > len(nums):
            return
        res.append(path[:])
        for i in range(start_index, len(nums)):
            path.append(nums[i])
            self.bt(nums, i+1, path, res)
            path.pop()


    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []
        self.bt(nums, 0, path, res)
        return res
```

[leetcode 90](https://leetcode.com/problems/restore-ip-addresses/)

直接写的时候忘了要排序，以及终止条件是i>start_index的条件下重复

```Python
class Solution:
    def bt(self, nums, start_index, path, res):
        if start_index > len(nums):
            return
        
        
        res.append(path[:])


        for i in range(start_index, len(nums)):
            if i > start_index and nums[i] == nums[i-1]:
                continue
            path.append(nums[i])
            self.bt(nums, i+1, path, res)
            path.pop()

    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []
        nums.sort()
        self.bt(nums, 0, path, res)
        return res
```
