# 代码随想录Day7

leetcode 454, 383, 15, 18

## [leetcode 454](https://leetcode.com/problems/4sum-ii/)

这个题有点没思路，但是看了一下很快就懂了，确实用哈希表存储有的数值是最好的
这个地方dict本身应该也可以，但是defaultdict明显更简洁，以后还是按这个走, 然后要搞清楚c+d必须是负值
同时d = collections.defaultdict(int)这种建立dict的方法也要记住，不要每次都直接import

```Python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        count = 0

        d = collections.defaultdict(int)
        for i in nums1:
            for j in nums2:
                d[i+j] += 1

        for k in nums3:
            for l in nums4:
                count += d[-(k+l)]

        return count
```

## [leetcode 383](https://leetcode.com/problems/ransom-note/)
这里面我的理解出现了问题
我的这种写法，只要有一个正确的就会确认True, 但实际上每个都要检查到，这个地方的条件处理错了
```Python
for ch in c1:
    if c2[ch] >= c1[ch]:
        return True     # ← 只要发现一个字母满足条件就直接 True
return False
```
正确ac写法
```Python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        if len(ransomNote) > len(magazine): 
            return False
        c1 = collections.Counter(ransomNote)
        c2 = collections.Counter(magazine)

        for i in c1:
            if c2[i] < c1[i]:
                return False
        
        return True
```
## [leetcode 15](https://leetcode.com/problems/3sum/)

这个题用双（三）指针明显是最好的解法，这里面的问题主要在于边界条件的处理
想明白 1. 出现重复怎么去重 2. 指针什么时候移动 3.去重和移动边界条件的关系

这个地方也可以用字典，用python会更简单，相当于两个指针移动找第三个，类似之前的四元祖

重复检查要用while
```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        for i in range (0, len(nums)):
            if nums[i] > 0:
                return result
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left = i+1
            right = len(nums) - 1
            while right > left:
                flag = nums[i] +  nums[left] + nums[right]
                if flag > 0:
                    right -=1
                elif flag < 0:
                    left +=1
                else:
                    result.append([nums[i], nums[left], nums[right]]) 

                    while right>left and nums[right] == nums[right-1]:
                        right -= 1
                    while right>left and nums[left] == nums[left +1]:
                        left += 1 

                    right -= 1 
                    left += 1

        return result
```
## [leetcode 18](https://leetcode.com/problems/ransom-note/)

```Python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        result = []
        for i in range(n):
            if nums[i] > target and nums[i] > 0 and target > 0:# 剪枝（可省）
                break
            if i > 0 and nums[i] == nums[i-1]:# 去重
                continue
            for j in range(i+1, n):
                if nums[i] + nums[j] > target and target > 0: #剪枝（可省）
                    break
                if j > i+1 and nums[j] == nums[j-1]: # 去重
                    continue
                left, right = j+1, n-1
                while left < right:
                    s = nums[i] + nums[j] + nums[left] + nums[right]
                    if s == target:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
                    elif s < target:
                        left += 1
                    else:
                        right -= 1
        return result

```
