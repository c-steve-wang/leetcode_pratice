# 代码随想录Day22

leetcode 77, 216, 17

[leetcode 77](https://leetcode.com/problems/combinations/)

这个题听力思路之后就自己写出来了，其实差不多已经想到了，还是比较简单的回溯
但是这个题这里还是出错了，因为global变量的问题，leetcode中还是要谨慎，其实其他的也是，避免global变量被清理不掉
最好让 res/path 的生命周期只在单个函数调用中，或者定义成 __int__()
全局列表会在所有测试用例之间共享，导致结果被反复累积
最后还要注意res.append(path)是不对的，必须是code中的写法

```Python
class Solution:
    def backtracking(self, n, k, start_index,path ,res):
        if len(path) == k:
            res.append(path[:])
            return
        
        for i in range(start_index, n+1):
            path.append(i)
            self.backtracking(n, k, i+1,path, res)
            path.pop()

        
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        path = []
        self.backtracking(n,k,1, path,res)
        return res 

```
剪枝实际上是至多的起始位置，而不是单纯的最后一个，这个地方要分清，尤其是树型结构往下延伸的时候

[leetcode 216](https://leetcode.com/problems/combination-sum-iii/)
这个地方一开始start_index 写错了，基本和上面的题一样
这个地方如果剪枝和上面那道题一样的方法，但是只有9个数据，减不减都一样
等于的情况下两种判断方式在这里都可以
但是注释里面的方法自带了剪枝，这个在后面要注意

```Python
class Solution:
    def bc(self, k, cur_sum, target_sum, start_index,path, res):
        if cur_sum > target_sum:
            return
        # if len(path) == k:
        #     if cur_sum == target_sum:
        #         res.append(path[:])
        #     return
        if cur_sum == target_sum and len(path)==k:
            return res.append(path[:])
        for i in range(start_index,10):
            cur_sum += i
            path.append(i)
            self.bc(k,cur_sum,target_sum,i + 1,path, res)
            cur_sum -= i
            path.pop()
            

    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        path = []
        self.bc(k, 0, n, 1,path, res)
        return res
```

[leetcode 17](https://leetcode.com/problems/combination-sum-iii/)
这个题直接想出来了，用回溯法还是比较好理解的，也不需要剪枝
但是这里用数组而不是字典的思路没想到，以及一定要转化成字符！
而且要return，不然会无限递归，模板还是要记牢

```Python
class Solution:
    def __init__(self):
        self.lettermap = [
            "",     # 0
            "",     # 1
            "abc",  # 2
            "def",  # 3
            "ghi",  # 4
            "jkl",  # 5
            "mno",  # 6
            "pqrs", # 7
            "tuv",  # 8
            "wxyz"  # 9
        ]
        self.result = []
        self.s_path = []

    def bc(self, digits, index):
        if index ==len(digits):
            s = "".join(self.s_path)
            self.result.append(s)
            return 
            
        digit = int(digits[index])
        letters = self.lettermap[digit]

        for i in range(len(letters)):
            self.s_path.append(letters[i])
            self.bc(digits, index + 1)
            self.s_path.pop()
        

    def letterCombinations(self, digits: str) -> List[str]:
        if len(digits) == 0:
            return self.result
        self.bc(digits, 0)
        return self.result
```

