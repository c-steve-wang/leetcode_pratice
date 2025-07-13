# 代码随想录Day16

leetcode 513, 112, 106

## [leetcode 513](https://leetcode.com/problems/find-bottom-left-tree-value/)

递归法里面最主要的还是回溯的过程，这个地方因为深度是一个变量，左右子树的深度应该是相同的，所以必须要回溯
回溯的地方可以先加再减，也可以先减再加

这个地方用层序遍历只需要不断更新保留i==0

最后还是写递归法，看起来考的比较多

```Pyhton
class Solution:
    def travelsal(self, node, depth):
        if not node.left and not node.right:
            if depth > self.max_depth:
                self.max_depth = depth
                self.result = node.val
            return

        if node.left:
            depth += 1
            self.travelsal(node.left, depth)
            depth -= 1
        if node.right:
            depth +=1
            self.travelsal(node.right, depth)
            depth -= 1


    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        self.max_depth = float('-inf')
        self.result = None
        self.travelsal(root, 0)

        return self.result
```
## [leetcode 112](https://leetcode.com/problems/path-sum/)
这个题确实想到了，但是没有想到减法更好做回溯，这个是真没想到。
最后主函数里面的值要减去根节点值，因为我们只考虑了左右子树的情况，没必要考虑中间节点
```Python
lass Solution:
    def travelsal(self, node, count):
        if not node.left and not node.right and count == 0:
            return True
        if not node.left and not node.right:
            return False
        
        if node.left:
            count -= node.left.val
            if self.travelsal(node.left,count):
                return True
            count += node.left.val
        
        if node.right:
            count -= node.right.val
            if self.travelsal(node.right,count):
                return True
            count += node.right.val
        
        return False


    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if root is None:
            return False
        
        return self.travelsal(root, targetSum - root.val)
```
## [leetcode 106](https://leetcode.com/problems/find-bottom-left-tree-value/)

这个地方要想明白递归怎么写实际上就是区分数组
但是问题在于一个结点具体该怎么定义，递归上又该怎么递归，尤其是最后两句没想到不应该

```Python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not postorder:
            return None

        root_val = postorder[-1]
        root = TreeNode(root_val)

        in_idx = inorder.index(root_val)
        in_left = inorder[:in_idx]
        in_right = inorder[in_idx + 1:]

        post_left = postorder[:len(in_left)]
        post_right = postorder[len(in_left):len(postorder) - 1]

        root.left = self.buildTree(in_left, post_left)
        root.right = self.buildTree(in_right, post_right)

        return root
```



