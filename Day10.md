# 代码随想录Day10

leetcode 232, 225, 20, 1047

## [leetcode 232](https://leetcode.com/problems/implement-queue-using-stacks/)

这个地方我能想到怎么做，但是用python实现确实是个问题
然后要注意面向对象的思想，已经定义过的函数就应该直接用

```Python
class MyQueue:

    def __init__(self):
        self.stack_in = []
        self.stack_out = []
        

    def push(self, x: int) -> None:
        self.stack_in.append(x)
        

    def pop(self) -> int:
        if self.empty():
            return None
        
        if self.stack_out:
            return self.stack_out.pop()
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()
        

    def peek(self) -> int:
        ans = self.pop()
        self.stack_out.append(ans)
        return ans
        

    def empty(self) -> bool:
        return not (self.stack_out or self.stack_in)

```

## [leetcode 225](https://leetcode.com/problems/implement-stack-using-queues/)

这个地方第一时间没反应上来，而且对python中deqeue也不是很熟悉，这个题不难理解但是要认真学习啊
先记录一个直接用双端队列的偷懒写法

```Python
class MyStack:

    def __init__(self):
        self.que = deque()
        

    def push(self, x: int) -> None:
        self.que.append(x)
        

    def pop(self) -> int:
        return self.que.pop()
        

    def top(self) -> int:
        if self.empty():
            return None

        return self.que[-1]
        

    def empty(self) -> bool:
        return not self.que
```

然后才是正常的用单端队列的解法
```Python
class MyStack:

    def __init__(self):
        self.que = deque()
        

    def push(self, x: int) -> None:
        self.que.append(x)
        

    def pop(self) -> int:
        if self.empty():
            return None
        for i in range(len(self.que)-1):
            self.que.append(self.que.popleft())
        return self.que.popleft()
        

    def top(self) -> int:
        if self.empty():
            return None
        for i in range(len(self.que)-1):
            self.que.append(self.que.popleft())
        ans = self.que.popleft()
        self.que.append(ans)
        return ans
        

    def empty(self) -> bool:
        return not self.que
```
## [leetcode 20](https://leetcode.com/problems/valid-parentheses/)

这个题也算是没用栈的方式想出来，这个里面要注意循环外和循环内判断为空的条件不一致，第一个是还在循环里面，后面有多的括号， 外面才是遍历结束为空。

这个地方也可以用python字典，相当于替代了刚开始的一圈elif

```Python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for item in s:
            if item == '(':
                stack.append(')')
            elif item == '{':
                stack.append('}')
            elif item == '[':
                stack.append(']')
            elif not stack or item != stack[-1]:
                return False
            else:
                stack.pop()
        
        if not stack:
            return True
        else:
            return False
```

## [leetcode 1047](https://leetcode.com/problems/valid-parentheses/)
这个题比较简单，也是一下就想到了，但是实现的时候是有一点小问题的
1. 首先我没有考虑到空队列的情况，这个要存在才能判断
2. 其次，pop（）这些函数是针对list的，不是针对字符串的，这个地方没有判断到

其他的没什么问题，这个题没看题解也想到了

```Python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list()
        for i in s:
            if res and res[-1] == i:
                res.pop()
            else:
                res.append(i)

        return ''.join(res)
```
