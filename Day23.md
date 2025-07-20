# 代码随想录Day23

leetcode 39,40,131

[leetcode 39](https://leetcode.com/problems/combination-sum/)

这个题是模板题，直接过，注意start index的位置可以重复，然后剪枝的话就是排序，如过已经大于target直接break

```Python
def backtracking(self, candidates, target, total, startIndex, path, result):
        if total > target:
            return
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i, path, result)  
            total -= candidates[i]
            path.pop()

    def combinationSum(self, candidates, target):
        result = []
        self.backtracking(candidates, target, 0, 0, [], result)
        return result
```

[leetcode 40](https://leetcode.com/problems/combination-sum-ii/)

其实这个地方我觉得用used数组有点多余而且抽象，反而对我来说容易绕进去。
其实说白了就是搜索树里面同一层去重，排序，然后第一遍可以搜，但是第二次搜到的时候就跳过去
used数组感觉就是辅助理解的，实际上在我这没什么用

```Python
class Solution:
    def bt(self, candidates, target, cur_sum, start_index,path, res):
        if cur_sum == target:
            res.append(path[:])
        for i in range(start_index, len(candidates)):
            if i > start_index and candidates[i] == candidates[i-1]:
                continue
            
            if cur_sum + candidates[i]> target:
                break

            cur_sum += candidates[i]
            path.append(candidates[i])
            self.bt(candidates, target, cur_sum, i + 1,path, res)
            path.pop()
            cur_sum -= candidates[i]

    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        path =[]
        candidates.sort()
        self.bt(candidates,target, 0, 0, path, result)
        return result
```
[leetcode 131](https://leetcode.com/problems/combination-sum-ii/)

这个题的难点主要是怎么分割的问题，其实就是画线画出一个树形结构，判断条件可以用来做剪枝，感觉上还是比较容易理解思路的，但是具体的细节，尤其是左闭右闭区间表示切割子串这点

```Python
class Solution:
    def is_pa(self,s, start, end):
        s = s[start:end+1]
        s1 = list(s)
        s2 = list(s)
        s2.reverse()
        if s1 == s2:
            return True
        else:
            return False

    def bt(self, s, start_index, path, res):
        if start_index == len(s):
            res.append(path[:])
            return
        
        for i in range(start_index, len(s)):
            if self.is_pa(s,start_index, i):
                path.append(s[start_index:i+1])
                self.bt(s, i+1, path, res)
                path.pop()

    def partition(self, s: str) -> List[List[str]]:
        res = []
        path = []
        self.bt(s,0,path, res)
        return res
```
最后注意一下，reverse只是就地操作，不能直接赋值，不如用-1




