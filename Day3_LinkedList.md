# 链表基础
链表的定义：

```python
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```

# <font style="color:#1a1a1a;">203.移除链表元素</font>
> 题目链接：[203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html)
>

**本题的关键**：需要通过设置一个**虚拟头结点**来统一对于头结点和非头结点的处理，不然的话如果头结点需要删除的话，需要更新头指针

即**链表操作的两种方式：**

1. 直接使用原来的链表来进行操作
2. 设置一个虚拟头结点在进行操作

错误记录：

```plain
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        ListNode* p = head C++的写法
        while(p.next != null): C++的写法
            if(p.val == val):  该比较的是下一个节点的val
                p.next = p.next.next
            p = p.next
    return p
没有考虑头结点如果需要删除的话需要特殊处理，更新头指针
```

正确：

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head
        p = dummy

        while p and p.next is not None:
            if p.next.val == val:
                p.next = p.next.next
            else: p = p.next

        return dummy.next
```

# 707.设计链表
> 题目链接：[707. 设计链表 - 力扣（LeetCode）](https://leetcode.cn/problems/design-linked-list/submissions/599708242/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)
>

就是对链表的增删改查

后续尝试一下利用双链表实现

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyLinkedList:
    def __init__(self):
        self.dummy = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        while index < 0 or index >= self.size:
            return -1
        
        p = self.dummy.next
        for _ in range(index):
            p = p.next
        
        return p.val

    def addAtHead(self, val: int) -> None:
        self.dummy.next = ListNode(val,self.dummy.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        p = self.dummy
        while p.next:
            p = p.next
        p.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        p = self.dummy
        if index < 0  or index > self.size:
            return 
        
        for _ in range(index):
            p = p.next
        p.next = ListNode(val, p.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        p = self.dummy
        for _ in range(index):
            p = p.next
        p.next = p.next.next 
        self.size -= 1
```

容易忽略之处：

+ `**<font style="color:#DF2A3F;">get</font>**`**<font style="color:#DF2A3F;"> 函数</font>**<font style="color:#DF2A3F;">应该从 </font>`<font style="color:#DF2A3F;">self.dummy.next</font>`<font style="color:#DF2A3F;"> 开始</font>，因为要访问链表中的实际节点，跳过虚拟头节点。
+ 在其他函数中，使用 `self.dummy` 开始遍历是合适的

时间复杂度: 涉及 `index` 的相关操作为 O(index), 其余为 O(1)

空间复杂度: O(n) 主要来源于链表节点

# 206.反转链表
> 题目链接：[206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
>

![](https://cdn.nlark.com/yuque/0/2025/gif/49293158/1739636778521-5f54bd71-776f-4482-a2f0-68e4fd36d5d3.gif)

后续研究下递归方法

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while cur:
            temp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = temp
        return pre
```

<font style="color:rgb(44, 62, 80);">时间复杂度: O(n)</font>

<font style="color:rgb(44, 62, 80);">空间复杂度: O(1)</font>

