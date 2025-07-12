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

## [leetcode 257](https://leetcode.com/problems/binary-tree-paths/)
这个题在回溯和递归的问题上有一点绕，要想清楚递归函数怎么操作，回溯又是怎么操作，这点比较重要，其他的部分还是蛮好理解的

```Python
class Solution:
    def travelsal(self, cur, path, res):
        path.append(cur.val)
        if not cur.left and not cur.right:
            sPath = '->'.join(map(str, path))
            res.append(sPath)
            return
        if cur.left:
            self.travelsal(cur.left,path, res)
            path.pop()
        if cur.right:
            self.travelsal(cur.right, path, res)
            path.pop()
    
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        res = []
        path = []
        if not root:
            return res
        
        self.travelsal(root, path,res)
        return res
```
## [leetcode 404](https://leetcode.com/problems/binary-tree-paths/)
这个题主要是从递归函数的角度理解，无论怎么样都会遍历到这个条件，而且把这个判断放在后面更能ac也更好理解, 但实际上没有任何区别，因为递归的时候也会去找这些条件

```Python 
class Solution:
    def sumOfLeftLeaves(self, root):
        if root is None:
            return 0
        if root.left is None and root.right is None:
            return 0
        leftValue = self.sumOfLeftLeaves(root.left)  
        if root.left and not root.left.left and not root.left.right:  
            leftValue = root.left.val

        rightValue = self.sumOfLeftLeaves(root.right)  

        sum_val = leftValue + rightValue  
        return sum_val
```

## [leetcode 222](https://leetcode.com/problems/binary-tree-paths/)
这个地方要注意深度是从1开始算的和从0开始算的条件下计算公式是不一样的，和里面移位有点差别
```Python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        l_node = root.left
        r_node = root.right
        l_count = 0
        r_count = 0

        while l_node:
            l_node = l_node.left
            l_count += 1
        while r_node:
            r_node = r_node.right
            r_count += 1
        
        if l_count == r_count:
            return 2**(l_count+1)-1 

        leftall = self.countNodes(root.left)
        rightall = self.countNodes(root.right)

        return leftall + rightall + 1
```
