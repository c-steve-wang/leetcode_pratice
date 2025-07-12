# 代码随想录Day15

leetcode 110,257,404, 222

## [leetcode 110](https://leetcode.com/problems/balanced-binary-tree/)

听明白之后还是很好理解的，主要是逐层返回 -1 这点很巧妙但是没有想到，听明白之后自己写完了剩下的代码然后ac了

```Python
class Solution:
    def getHeight(self,node):
        if not node:
            return 0
        res = 0
        leftheight = self.getHeight(node.left)
        if leftheight == -1:
            return -1
        rightheight = self.getHeight(node.right)
        
        if rightheight == -1:
            return -1
        
        if abs(leftheight - rightheight) > 1:
            res = -1
        else:
            res = 1 + max(leftheight, rightheight)

        return res
        
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        
        result = self.getHeight(root)

        return True if result >= 1 else False
```
