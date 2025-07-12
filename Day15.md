# 代码随想录Day15

leetcode 110,257,404, 222

## [leetcode 110](https://leetcode.com/problems/invert-binary-tree/)

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
