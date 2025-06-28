# 代码随想录Day3

leetcode 203, 707, 206

这部分基本是知道原理但是具体的细节不是很清楚，尤其是用python一步步实现

## [leetcode 203](https://leetcode.com/problems/remove-linked-list-elements/)

这个地方的基本原理我是清楚的，但是还是犯了两个错误要注意

1. 不可能上来用if，用if的话不可能遍历全部的节点，所以必须要用val
2. 其次，这个地方要学会用dummy head，似乎python只有这种解题方法
3. 最后要返回dummy.next 不要返回head，因为head可能已经没了
4. 不要忘了while 不会自动循环，cur指针要往后更新...


```Python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head

        prev, cur = dummy, head
        while cur:
            if cur.val == val:
                prev.next = cur.next
            else:
                prev = cur
            cur = cur.next
            
        return dummy.next

```

## [leetcode 707](https://leetcode.com/problems/reverse-linked-list/)

1. 新建一个结点就是ListNode()这个要记住
2. 所有的add要从虚拟节点开始，因为你并不知道现在的是不是空的，有可能tail就是head。注意在结尾只需要ListNode(val)，因为没有下一个了
3. cur.next = ListNode(val, cur.next) 先执行里面的到后一个结点，最后再更新前结点的next
4. 注意检查index的边界条件，addatIndex是唯一可以等于数组长度的，因为可以放在最后虚空索敌，但是其他两个必须要存在 

```Python
class MyLinkedList:

    def __init__(self):
        self.size = 0
        self.dummy = ListNode(0)

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        cur = self.dummy.next
        for i in range(index):
            cur = cur.next

        return cur.val

    def addAtHead(self, val: int) -> None:
        self.dummy.next = ListNode(val, self.dummy.next)
        self.size += 1
        

    def addAtTail(self, val: int) -> None:
        cur = self.dummy

        while cur.next:
            cur = cur.next

        cur.next = ListNode(val)
        self.size += 1    

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return -1
        cur = self.dummy

        for i in range(index):
            cur = cur.next
        
        cur.next = ListNode(val, cur.next)
        self.size += 1
        

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return -1
        
        cur = self.dummy
        for i in range(index):
            cur = cur.next
        cur.next = cur.next.next
        self.size -= 1

```

## [leetcode 206](https://leetcode.com/problems/remove-element/)

这个地方我还是喜欢双指针，递归也看明白了，但还是喜欢时间复杂度和空间复杂度更低的写法
主要还是这些思路比较熟悉，巩固一下，主要的问题还是在细节的实现上
而且python比之前c++ 简单了不少，考虑的也可以少一点

```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None
        cur = head
        while cur:
            temp = cur.next
            cur.next = pre
            pre = cur
            cur = temp
        
        return pre
        
```

