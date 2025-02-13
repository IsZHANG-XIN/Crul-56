# 704. 二分查找
:::tips
<font style="color:#333333;">题目链接：</font>[704. 二分查找 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-search/)

<font style="color:#333333;">文章讲解：</font>[代码随想录](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)

<font style="color:#333333;">视频讲解：</font>[手把手带你撕出正确的二分法 | 二分查找法 | 二分搜索法 | LeetCode：704. 二分查找_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1fA4y1o715)

:::

二分查找的核心点：**区间的概念**

    通常有两种区间定义：左闭右闭 [left,right] 和 左闭右开 [left,right）  
    二分查找细节容易出错就是容易在更新搜索区间时的边界条件怎么写，保持正确的方法就是根据自己遵循的区间去写相应的边界条件

【python版本】

有效搜索区间定义为左闭右闭：

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1  # 定义target在左闭右闭的区间里，[left, right]
        while left <= right:
            middle = left + (right - left) // 2

            if nums[middle] > target:
                right = middle - 1  # target在左区间，所以[left, middle - 1]
            elif nums[middle] < target:
                left = middle + 1  # target在右区间，所以[middle + 1, right]
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值
```

 分析：

right初始化：由于定义的区间是左闭右闭，right就是该等于最后一个元素的下标，这样left和right所夹的区间才是符合题设要求的区间。如果写成len(nums)则意味着搜索区间会多出来一个不属于nums数组的非法单元

进入while循环的条件：由于区间是左闭右闭，则意味着当left恰好等于right时，也是合法的，也该进行搜索，即[1,1]是合法区间，left和right指向同一个位置时，这个位置的元素也应该被搜索。

更新搜索区间的规则：如果当前middle值大于target，则应该更新左区间的右边界，由于已知middle的值是小于target的，并且搜索区间是闭区间，所以新的搜索区间不应该包括middle这个值了，所以right需要更新为middle-1。如果当前middle值小于target，同理。

有效搜索区间定义为左闭右闭：

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)  # 定义target在左闭右开的区间里，即：[left, right)
        while left < right:  # 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            middle = left + (right - left) // 2

            if nums[middle] > target:
                right = middle  # target 在左区间，在[left, middle)中
            elif nums[middle] < target:
                left = middle + 1  # target 在右区间，在[middle + 1, right)中
            else:
                return middle  # 数组中找到目标值，直接返回下标
            return -1  # 未找到目标值
```

分析：

进入while的条件：即while的条件应该是表示的合法的搜索区间，即left小于right时，如果left=right，那会造成非法区间的概念，如[1,1)是非法的。

更新搜索区间的规则：如果当前middle值大于target，应更新right，并且right应该等于middle，因为下一个搜索区间是不会包括middle的，而middle确实也已经被判出局了，是没问题的。如果当前middle值小于target，还有些说法，此时要更新的是left，因为left这端是闭区间，所以应该更新为middle+1，不然就会重复搜索middle这个位置。

新知：

循环不变量规则：区间的定义就是不变量，那么在循环中坚持根据查找区间的定义来做边界处理，就是循环不变量规则。

本题注意点：

数组内元素是有序的。

---

# 27. 移除元素
<font style="color:#333333;">题目链接：</font>[27. 移除元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-element/)

<font style="color:#333333;">文章讲解：</font>[代码随想录](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)

<font style="color:#333333;">视频讲解：</font>[数组中移除元素并不容易！ | LeetCode：27. 移除元素_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV12A4y1Z7LP)

【解法一】双向指针法

自己第一遍写的错误版本：

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        left = 0
        right = len(nums)-1
        while left<=right:
            if(nums[left] == val): nums[right] = nums[left]
            left = left + 1
        k = len(nums)
        return k
```

自己想到的是使用双指针做交换完成，改正后可以通过：

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        left = 0
        right = len(nums)-1
        while left<=right:
            if nums[left] == val: 
                nums[left] = nums[right]
                right -= 1
            else:
                left += 1
        return left
```

【解法二】快慢指针（推荐⭐）

双指针法（快慢指针法）： 通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。

定义快慢指针

快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组  
慢指针：指向更新 新数组下标的位置

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        # 快慢指针
        fast = 0  # 快指针
        slow = 0  # 慢指针
        size = len(nums)
        while fast < size:  # 不加等于是因为，a = size 时，nums[a] 会越界
            # slow 用来收集不等于 val 的值，如果 fast 对应值不等于 val，则把它与 slow 替换
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```

  
【解法三】暴力解-两重for循环

class Solution:  
    def removeElement(self, nums: List[int], val: int) -> int:  
        i, l = 0, len(nums)  
        while i < l:  
            if nums[i] == val: # 找到等于目标值的节点  
                for j in range(i+1, l): # 移除该元素，并将后面元素向前平移  
                    nums[j - 1] = nums[j]  
                l -= 1  
                i -= 1  
            i += 1  
        return l  
977.有序数组的平方  
题目链接：977. 有序数组的平方 - 力扣（LeetCode）

文章讲解：代码随想录

视频讲解： 双指针法经典题目 | LeetCode：977.有序数组的平方_哔哩哔哩_bilibili

 【解法一】双指针法



class Solution:  
    def sortedSquares(self, nums: List[int]) -> List[int]:  
        l, r, i = 0, len(nums)-1, len(nums)-1  
        res = [float('inf')] * len(nums) # 需要提前定义列表，存放结果  
        while l <= r:  
            if nums[l] ** 2 < nums[r] ** 2: # 左右边界进行对比，找出最大值  
                res[i] = nums[r] ** 2  
                r -= 1 # 右指针往左移动  
            else:  
                res[i] = nums[l] ** 2  
                l += 1 # 左指针往右移动  
            i -= 1 # 存放结果的指针需要往前平移一位  
        return res  
【解法二】暴力排序法+列表推导法

class Solution:  
    def sortedSquares(self, nums: List[int]) -> List[int]:  
        return sorted(x*x for x in nums)  
 用时：3h  
————————————————

```plain
                        版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
                    
```

原文链接：[https://blog.csdn.net/2401_87221149/article/details/145620996](https://blog.csdn.net/2401_87221149/article/details/145620996)

