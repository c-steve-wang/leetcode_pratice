# 代码随想录Day14

leetcode 226, 101, 104, 111

## [leetcode 226](https://leetcode.com/problems/invert-binary-tree/)

这个题是很简单很好理解的，但是在python实现的过程中间我还是有一些薄弱的地方
比如swap这个操作在python里面怎么操作，迭代法的话需要self.fucntion而不是单纯的call function

```Python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)

        return root
```
## [leetcode 101](https://leetcode.com/problems/symmetric-tree/)
这个题一定不能用or因为三个条件并不等价，其次，比较函数里面的left和right只是一种变量表示，并不代表实际意义，不要强行解读
最后要理解这个后序的思路，先处理左右孩子，然后把值返回上一层，再辅助更上一层判断
最后还有个小坑就是如果整个树是空的那么一定是true这个和之前的不同

```Python
class Solution:
    def compare(self, left, right):
        if left == None and right != None:
            return False
        elif left != None and right == None:
            return False
        elif left == None and right == None:
            return True
        elif left.val != right.val:
            return False
        outside = self.compare(left.left, right.right)
        inside = self.compare(left.right, right.left)
        res = outside and inside
        return res

    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        return self.compare(root.left,root.right)
        
```


## [leetcode 104](https://leetcode.com/problems/invert-binary-tree/)
这道题理解了思路还是蛮简单的，而且总体上来说也比之前更熟悉python的写法，最后这道题一定要注意为什么用后序
还是之前的那个思想，先处理左右，返回到中间，让中间的处理

```Python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        lefthight = self.maxDepth(root.left)
        righthight = self.maxDepth(root.right)

        height = 1 + max(lefthight, righthight)

        return height
```



## [leetcode 111](https://leetcode.com/problems/invert-binary-tree/)
这个地方的叶子节点要注意，只有左右都空才是叶子结点，这个是隐藏的雷电

```Python
class Solution:
    def getDepth(self, node):
        if node is None:
            return 0
        leftDepth = self.getDepth(node.left)  
        rightDepth = self.getDepth(node.right)  
        
        if node.left is None and node.right is not None:
            return 1 + rightDepth
        
        if node.left is not None and node.right is None:
            return 1 + leftDepth
        
        result = 1 + min(leftDepth, rightDepth)
        return result

    def minDepth(self, root):
        return self.getDepth(root)

```
