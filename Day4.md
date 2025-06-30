# 代码随想录Day4

leetcode 24，19，160， 142

## [leetcode 24](https://leetcode.com/problems/swap-nodes-in-pairs/)

这个题想到了大部分，但最后的问题在于没画图，具体的指针变化有点晕
条件少考虑的点，不用定义pre，一个cur就够了，但是cur.next 和 cur.next.next 都要存在（这个地方考虑结点总数奇数或者偶数的情况，或者直接考虑两个示例里面的）


```Python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head

        cur = dummy

        while cur.next and cur.next.next:
            temp1 = cur.next
            temp2 = cur.next.next.next

            cur.next = cur.next.next
            cur.next.next = temp1
            temp1.next = temp2
            cur = cur.next.next
        return dummy.next 

```

## [leetcode 19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

第一个自己写的ac了，简单来说就是先算长度，然后根据长度算需要从空结点向后几次，找到删除结点的前一个结点直接删就行

```Python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head
        length = 0
        cur = head
        while cur:
            length +=1
            cur = cur.next
        cur1 = dummy
        for i in range(length - n):
            cur1 = cur1.next

        cur1.next = cur1.next.next
        return dummy.next

```

第二个一开始隐约想到了，但是思路一开始没有想清楚，没有想清楚用快慢指针和快n+1步的判断条件，但是听完思路之后就开悟了,直接写ac

```Python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head
        
        fast, slow = dummy, dummy
        for i in range(n+1):
            fast = fast.next

        while fast:
            fast = fast.next
            slow = slow.next
        
        slow.next = slow.next.next

        return dummy.next

```

## [leetcode 160](https://leetcode.com/problems/intersection-of-two-linked-lists/)

这个地方需要注意两点
1. 指针相等不是数值相等，因为next指针的地址可能不一样，实际上是后面的所有地址都必须一样。单纯的数值相等不一定找到这个位置
2. 要主要不能用abs，这个地方要确保先动长链表，用abs不能显示这一点
3. 等比例法看着很优雅但是确实面试的时候不容易证明，不如直接说这个

```Python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        lenA, lenB = 0, 0 
        cur = headA
        while cur:
            cur = cur.next
            lenA +=1
        
        cur = headB
        while cur:
            cur = cur.next
            lenB += 1

        curA = headA
        curB = headB
        
        if lenB < lenA:
            curA, curB = curB,curA
            lenA, lenB = lenB,lenA
        
        for _ in range(lenB - lenA):
            curB = curB.next

        while curA:
            if curA == curB:
                return curA
            else:
                curA = curA.next
                curB = curB.next
        
        return None
```
## [leetcode 142](https://leetcode.com/problems/linked-list-cycle-ii/)

这个地方最终的是公式证明
设快指针走两步，慢指针走一步，x到入口，y从入口到相遇，z是从相遇到入口

→ 2(x+y) = x + y + n(y + z)

→ x = (n-1)y + z

→ x = z 

最后一步因为走了几圈并不关键，关键的是从相遇点到入口，和从头到入口一致，到这一步就证明完毕了
主要是把这一步推出来就好了，其他的没什么问题

```Python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast, slow = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                index1 = fast
                index2 = head
                while index1 != index2:
                    index1 = index1.next
                    index2 = index2.next
                return index1 
```
