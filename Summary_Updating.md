# 数组操作中常用到的方法
## 二分法
循环不变量原则：把握循环条件正确的关键

左闭右闭

左闭右开

## 双向指针
交换，有点像快排的思想

## 双指针变体-快慢指针
通过一个快指针和慢指针在一个for循环下完成两个for循环的工作

时间复杂度：O(n)

## 双指针变体-滑动窗口
理解滑动窗口如何移动 窗口起始位置，达到动态更新窗口大小的，从而得出长度最小的符合条件的长度。

时间复杂度：O(n)

## 模拟行为
## 前缀和
![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739543094444-5b32140d-1277-493f-bf5b-bc0fc07d46d2.png)

# 链表
## 虚拟头结点
涉及到需要寻找该节点前一个节点，再进行操作的类型，最好都使用虚拟头节点，这样可以统一头节点和非头节点的处理。

## 链表的增删改查
## 双指针（多是快慢指针）是链表的常见方法
+ 反转链表
+ 删除倒数第 N 个节点
+ 链表相交
+ 环形链表

















## 代码类错误
### 列表不给出初始值直接使用会导致索引越界错误
错误写法：

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739540279234-39fcbcdf-ad05-4382-8d6d-76674d1c1dba.png)

正确：

应该先初始化列表，并给它们分配合适的空间

```python
h = [0] * n  # 给 h 分配一个大小为 n 的零列表
l = [0] * m  # 给 l 分配一个大小为 m 的零列表
```

