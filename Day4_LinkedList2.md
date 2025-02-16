# <font style="color:rgb(44, 62, 80);">24.两两交换链表中的节点</font>
> <font style="color:rgb(44, 62, 80);">题目链接：</font>[24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
>
> <font style="color:rgb(44, 62, 80);">讲解文章：</font>[代码随想录](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)
>

要求：给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。不能只是单纯的改变节点内部的值，而是**需要实际的进行节点交换**。

**<font style="color:rgba(43,61,79,1);">关键：</font>**

1. 本题属于模拟题，一定要**画图！**

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739710179495-cb27d072-6fb6-4aea-ae55-1fbe97307510.png)

2. current 指针要指向要交换的两个节点的前一个节点才可以实现反转

错误记录：

```plain
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head) 要把head赋值给dummy_head的next而不是直接给dummy_head，否则会导致破坏头结点而超时
        current = dummy_head

        while current.next  and current.next.next :
            temp1 = current.next
            temp2 = current.next.next.next 需要提前保存，否则该节点会丢失

            current.next = current.next.next
            current.next.next = temp1
            temp1.next = temp2

            current = current.next.next
        
        return dummy_head.next 返回的是虚拟头结点的next才是链表的头结点，而不是current.next，current的位置已经变了
```

正确：

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        current = dummy_head

        while current.next  and current.next.next :
            temp1 = current.next
            temp2 = current.next.next.next

            current.next = current.next.next
            current.next.next = temp1
            temp1.next = temp2

            current = current.next.next
        
        return dummy_head.next
```

递归解法：

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        # 待翻转的两个node分别是pre和cur
        pre = head
        cur = head.next
        next = head.next.next
        
        cur.next = pre  # 交换
        pre.next = self.swapPairs(next) # 将以next为head的后续链表两两交换
         
        return cur
```

# 19.删除链表的倒数第 N 个节点
题目链接：[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

讲解文章：[代码随想录](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

思路：使用**快慢指针**，<font style="color:rgb(44, 62, 80);">双指针的经典应用：如果要删除倒数第n个节点，先让fast移动n+1 步，然后让fast和slow同时移动，直到fast指向链表末尾。删掉slow所指向的节点就可以了。</font>

1. fast 指针和 slow 指针初始指向虚拟头结点
2. fast 先走 n+1 步，再让 fast 和 slow 同时走，知道 fast 指向 null，此时 slow 指向的就是待删除的倒数第 N 个节点的前一个节点，只有指向待删节点的前一个节点才能实现删除操作

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739712183850-2e736158-93bd-4ee4-8349-d04860dc19fc.png)

错误记录：

```plain
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        fast = dummy_head
        slow = dummy_head

        for _ in range(n+1):
            fast = fast.next
        while fast:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next 此操作要放在while外
        
        return dummy_head.next
```

正确：

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        fast = dummy_head
        slow = dummy_head

        for _ in range(n+1):
            fast = fast.next
        while fast:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next

        return dummy_head.next
```

时间复杂度：O(n)

空间复杂度：O(1)

# 面试题 02.07.链表相交
> 题目链接：[面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)
>

<font style="color:rgb(44, 62, 80);">给两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。</font>

<font style="color:rgb(44, 62, 80);">思想：还是使用双指针，先求链表各自的长度，让长的</font>**<font style="color:rgb(44, 62, 80);">先走长度之差</font>**<font style="color:rgb(44, 62, 80);">，对齐</font>**<font style="color:rgb(44, 62, 80);">剩余的长度</font>**<font style="color:rgb(44, 62, 80);">，再依次比对</font>**<font style="color:#DF2A3F;">节点（不是节点的 val！）</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739713953557-7b1ff36b-5a4c-47e1-8c1c-7f25a334450a.png)

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        # 计算链表长度
        lenA, lenB = 0, 0
        cur = headA
        while cur:
            cur = cur.next
            lenA += 1
        cur = headB
        while cur:
            cur = cur.next
            lenB += 1
        
        # 对齐两个链表的起始位置
        curA, curB = headA, headB
        if lenA > lenB:
            # 使得 curA 总是指向较短的链表
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA

        # 调整较长链表的指针
        for _ in range(lenB - lenA):
            curB = curB.next

        # 同步遍历两个链表
        while curA:         # 遍历curA 和 curB，遇到相同则直接返回
            if curA == curB:
                return curA
            else:
                curA = curA.next 
                curB = curB.next
```

<font style="color:rgb(44, 62, 80);">代码复用版本：</font>

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        dis = self.getLength(headA) - self.getLength(headB)
        
        # 通过移动较长的链表，使两链表长度相等
        if dis > 0:
            headA = self.moveForward(headA, dis)
        else:
            headB = self.moveForward(headB, abs(dis))
        
        # 将两个头向前移动，直到它们相交
        while headA and headB:
            if headA == headB:
                return headA
            headA = headA.next
            headB = headB.next
        
        return None
    
    def getLength(self, head: ListNode) -> int:
        length = 0
        while head:
            length += 1
            head = head.next
        return length
    
    def moveForward(self, head: ListNode, steps: int) -> ListNode:
        while steps > 0:
            head = head.next
            steps -= 1
        return head
```

# 142.环形链表 II
> 题目链接：[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)
>

<font style="color:rgb(44, 62, 80);">题意： 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。</font>

<font style="color:#00346B;">本题其实有两问：</font>

1. <font style="color:#00346B;">如何判断链表是否有环</font>
2. <font style="color:#00346B;">如果有环，如果确定入口位置</font>

如何判断链表是否有环：

使用快慢指针，同时从头结点出发，快指针每次都 2 个，慢指针每次走 1 个，如果没有环的话，快慢指针永远不可能相遇；如果相遇，必然是有环，且一定是在环内相遇，因为快慢指针之所以会相遇就是因为快指针追上了慢指针。所以，可以用快慢指针是否相遇来判断是否有环。

![](https://cdn.nlark.com/yuque/0/2025/gif/49293158/1739716607383-b8fc3391-7a01-4aae-a872-5330e9550862.gif)

确定了有环，如何确定环入口位置：

设变量：假设从头结点到环形入口节点 的节点数为x。 环形入口节点到 fast指针与slow指针相遇节点 节点数为y。 从相遇节点 再到环形入口节点节点数为 z。

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739716879595-f0ebbd9d-7b6e-4bf5-a601-b20ceab6c374.png)

那么相遇时： slow指针走过的节点数为: `x + y`， fast指针走过的节点数：`x + y + n (y + z)`，n为fast指针在环内走了n圈才遇到slow指针， （y+z）为 一圈内节点的个数A。

因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数 = slow指针走过的节点数 * 2：`(x + y) * 2 = x + y + n (y + z)`

两边消掉一个（x+y）: `x + y = n (y + z)`

因为要找环形的入口，那么要求的是x，因为x表示 头结点到 环形入口节点的的距离。

所以要求x ，将x单独放在左面：`x = n (y + z) - y` ,

再从n(y+z)中提出一个 （y+z）来，整理公式之后为如下公式：`x = (n - 1) (y + z) + z` 注意这里n一定是大于等于1的，因为 fast指针至少要多走一圈才能相遇slow指针。

这个公式说明什么呢？

先拿n为1的情况来举例，意味着fast指针在环形里转了一圈之后，就遇到了 slow指针了。

当 n为1的时候，公式就化解为 `x = z`，

这就意味着，**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。

也就是在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。

让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。

那么 n如果大于1是什么情况呢，就是fast指针在环形转n圈之后才遇到 slow指针。

其实这种情况和n为1的时候 效果是一样的，一样可以通过这个方法找到 环形的入口节点，只不过，index1 指针在环里 多转了(n-1)圈，然后再遇到index2，相遇点依然是环形的入口节点。

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739715715819-86d550dd-e14c-4fcd-bfae-be6ba4157692.png)

通过右图右下角，可以理解为什么一定会在满指针进入环内的第一圈就被快指针追上。

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        slow = head
        fast = head
        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
            # If there is a cycle, the slow and fast pointers will eventually meet
            if slow == fast:
                # Move one of the pointers back to the start of the list
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        # If there is no cycle, return None
        return None
```

时间复杂度：O(n)

空间复杂度：O(1)

