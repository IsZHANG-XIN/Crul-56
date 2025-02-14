<font style="color:#1a1a1a;">209.长度最小的子数组</font>

<font style="color:#1a1a1a;">59.螺旋矩阵II</font>

<font style="color:#1a1a1a;">区间和</font>

<font style="color:#1a1a1a;">开发商购买土地</font>

# 209.长度最小的子数组
> <font style="color:#333333;">题目链接：</font>[209. 长度最小的子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-size-subarray-sum/)
>
> <font style="color:#333333;">文章讲解：</font>[代码随想录](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html)
>
> <font style="color:#333333;">视频讲解：</font>[拿下滑动窗口！ | LeetCode 209 长度最小的子数组_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1tZ4y1q7XE)
>

## 【解法一】滑动窗口（双指针变体）推荐⭐
所谓滑动窗口，**就是不断的调节子序列的起始位置和终止位置，从而得出我们要想的结果**。

在暴力解法中，是一个for循环滑动窗口的起始位置，一个for循环为滑动窗口的终止位置，用两个for循环 完成了一个不断搜索区间的过程。

那么滑动窗口如何**用一个for循环**来完成这个操作呢

**<font style="background-color:#FBDE28;">滑动窗口中的关键问题</font>** ：

1. for 循环中的指针表示的是搜索区间的起始还是终止？

> 如果表示的是起始位置，那不可避免需要用另一个指针遍历后续的元素，那不还是暴力解吗
>

2. 确定了需要用 for 循环的索引来表示子数组的终止位置，那怎么逐渐缩小搜索区间以找到符合条件的最短区间？

<font style="color:rgba(43,61,79,1);">窗口就是 满足其和 ≥ s 的长度最小的 连续 子数组。</font>

<font style="color:rgba(43,61,79,1);">窗口的起始位置如何移动：如果当前窗口的值大于等于s了，窗口就要向前移动了（也就是该缩小了）。</font>

<font style="color:rgba(43,61,79,1);">窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。</font>

![](https://cdn.nlark.com/yuque/0/2025/gif/49293158/1739503447208-427f7886-6f60-4f86-a15f-5aefdd30e241.gif)

```python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        l = len(nums)
        left = 0
        right = 0
        min_len = float('inf')# 初始化为无穷大
        cur_sum = 0 #当前的累加值
        
        while right < l:
            cur_sum += nums[right]
            
            while cur_sum >= s: # 当前累加值大于目标值
                min_len = min(min_len, right - left + 1) #必须要加上，才能保证最后得到的是最短的子区间❗️
                cur_sum -= nums[left]
                left += 1
            
            right += 1
        
        return min_len if min_len != float('inf') else 0
```

+ 时间复杂度：O(n)

❓<font style="background-color:#FBDE28;">时间复杂度的解读</font>：由于 left 指针和 right 指针都最多移动 n 次，而且两个指针的移动是相互独立的，不会出现重复移动的情况。所以，总的操作次数最多是 2n 次。

+ 空间复杂度：O(1)

## 【解法二】暴力双重 for 循环
依次遍历所有的子数组

超时❌

O(n<sup>2</sup>)

```python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        l = len(nums)
        min_len = float('inf')
        
        for i in range(l):
            cur_sum = 0
            for j in range(i, l):
                cur_sum += nums[j]
                if cur_sum >= s:
                    min_len = min(min_len, j - i + 1)
                    break
        
        return min_len if min_len != float('inf') else 0
```

---

# <font style="color:#1a1a1a;">59.螺旋矩阵II</font>
**<font style="color:#333333;">本题关键</font>**<font style="color:#333333;">：转圈的逻辑，在二分搜索中提到的区间定义，在这里又用上了。</font>  


> <font style="color:#333333;">题目链接：</font>[59. 螺旋矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix-ii/)
>
> <font style="color:#333333;">文章讲解：</font>[代码随想录](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html)
>
> <font style="color:#333333;">视频讲解：</font>[一入循环深似海 | LeetCode：59.螺旋矩阵II_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1SL4y1N7mV/)
>

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739524276819-87736280-1e79-46f8-b0b4-5418d4c6a669.png)

核心之处就是本题涉及到非常多的边界条件，就像最初的二分搜索法一样，要遵循**循环不变量**。

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)]
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数

        for offset in range(1, loop + 1) :      # 每循环一层偏移量加1，偏移量从1开始
            for i in range(starty, n - offset) :    # 从左至右，左闭右开
                nums[startx][i] = count
                count += 1
            for i in range(startx, n - offset) :    # 从上至下
                nums[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左
                nums[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上
                nums[i][starty] = count
                count += 1                
            startx += 1         # 更新起始点
            starty += 1

        if n % 2 != 0 :			# n为奇数时，填充中心点
            nums[mid][mid] = count 
        return nums
```

   
在 for i in range(n - offset, starty, -1) 这段代码中，-1 是 range 函数的**步长**参数。**通过 step 为 -1 这个参数，使得循环变量 i 的值每次递减 1，从而实现了从右至左的遍历**。



这一行代码：

```python
nums = [[0] * n for _ in range(n)]
```

用于创建一个大小为 n×n 的二维列表。具体来说，它通过列表解析的方式创建了一个包含 n 行，每行有 n 个元素的二维列表，其中每个元素都初始化为 `0`。

> 1. `**[0] * n**`:
>     - 这个表达式创建了一个包含 `n` 个 `0` 的列表。例如，若 `n = 3`，则 `[0] * 3` 结果是 `[0, 0, 0]`。
>     - 这相当于创建了一个包含 n 个 `0` 的一维列表。
> 2. `**for _ in range(n)**`:
>     - 这是一个列表解析中的循环，`_` 是一个常用的占位符，表示循环变量在这里不需要使用。
>     - `range(n)` 生成一个从 0 到 n−1 的数字序列，循环执行 n 次。
>     - 每次循环时，`[0] * n` 被重新创建并加入到外部的列表中。
> 3. **最终的结构**:
>     - 外层的列表解析 `[... for _ in range(n)]` 表示生成一个包含 nnn 个元素的列表，每个元素是一个长度为 n 的一维列表。
>     - 因此，最终的 `nums` 变成了一个大小为 n×n 的二维列表，每个元素都初始化为 `0`。
>

<font style="color:rgb(44, 62, 80);">时间复杂度 O(n</font><sup><font style="color:rgb(44, 62, 80);">2</font></sup><font style="color:rgb(44, 62, 80);">): 模拟遍历二维矩阵的时间</font>

+ <font style="color:rgb(44, 62, 80);">每次填充一层，实际上会填充 4×(n−2×offset) 个元素。</font>
+ <font style="color:rgb(44, 62, 80);">由于每次填充的元素总数是 O(n)级别，外层循环控制了从外到内的层数，大致是 n/2 层（每次去掉一圈）。</font>
+ <font style="color:rgb(44, 62, 80);">因此，每个元素都会被访问一次。总的填充元素数目是 n</font><sup>2</sup><font style="color:rgb(44, 62, 80);">，即矩阵中每个元素都会被填充一次。</font>

<font style="color:rgb(44, 62, 80);">空间复杂度 O(1)</font>



---

# <font style="color:#1a1a1a;">区间和</font>
> 题目链接：[58. 区间和（第九期模拟笔试）](https://kamacoder.com/problempage.php?pid=1070)
>
> 讲解文章：[58. 区间和 | 代码随想录](https://www.programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html#%E6%80%9D%E8%B7%AF)
>



**<font style="color:#DF2A3F;">前缀和</font>****<font style="color:rgb(44, 62, 80);"> 在涉及计算区间和的问题时非常有用</font>**<font style="color:rgb(44, 62, 80);">！</font>

<font style="color:#000000;">暴力解法在面对大批量数据时会造成极高的时间复杂度：如果查询m次，每次查询的范围都是从0 到 n - 1，那么暴力算法的时间复杂度是 O(n * m) ，m 是查询的次数，如果查询次数非常大的话，这个时间复杂度也是非常大的。</font>

**<font style="color:rgb(44, 62, 80);">前缀和的思想：重复利用计算过的子数组之和，从而降低区间查询需要累加计算的次数。</font>**

申请一个新的数组，专门用来存储 0-i 区间和，后续计算 a-b 区间内的和就只需要做一次简单的减法即可，如图👇

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739537128363-35450f73-f07b-4970-84aa-893ab133ed8c.png)

```python
import sys
input = sys.stdin.read

def main():
    '''
    使用 sys.stdin.read() 来一次性读取所有的输入数据，并把它绑定到 input 变量上。
    这样可以避免逐行读取输入，从而提高输入的效率。读取后，数据会以字符串的形式返回。
    '''
    data = input().split()
    
    index = 0
    n = int(data[index])
    index += 1
    vec = []
    for i in range(n):
        vec.append(int(data[index + i]))
    index += n

    p = [0] * n
    presum = 0
    for i in range(n):
        presum += vec[i]
        p[i] = presum

    results = []
    while index < len(data):
        a = int(data[index])
        b = int(data[index + 1])
        index += 2

        if a == 0:
            sum_value = p[b]
        else:
            sum_value = p[b] - p[a - 1]

        results.append(sum_value)

    for result in results:
        print(result)

if __name__ == "__main__":
    main()
```

前缀和解法的优势：

+ **前缀和**：使用前缀和数组 `p` 来预先存储所有区间的累计和，查询时只需常数时间计算结果（即 O(1)）。
+ **查询优化**：每次查询通过简单的差值 `p[b] - p[a-1]` 就可以得到结果，避免了每次都遍历区间的开销。
+ **高效输入输出**：使用 `sys.stdin.read()` 一次性读取所有输入，减少了逐行读取的开销。

---

# <font style="color:rgb(44, 62, 80);">44. 开发商购买土地</font>
> 题目链接：[44. 开发商购买土地（第五期模拟笔试）](https://kamacoder.com/problempage.php?pid=1044)
>
> 讲解文章：[44. 开发商购买土地 | 代码随想录](https://www.programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html#%E6%80%9D%E8%B7%AF)
>

<font style="color:rgb(44, 62, 80);">思路：使用 前缀和的思路来求解，先将 行方向，和 列方向的和求出来，这样可以方便知道 划分的两个区间的和</font>

```python
def main():
    import sys
    input = sys.stdin.read
    data = input().split()

    idx = 0
    n = int(data[idx])
    idx += 1
    m = int(data[idx])
    idx += 1
    sum = 0
    vec = []
    for i in range(n):
        row = []
        for j in range(m):
            num = int(data[idx])
            idx += 1
            row.append(num)
            sum += num
        vec.append(row)

    # 统计横向
    horizontal = [0] * n
    for i in range(n):
        for j in range(m):
            horizontal[i] += vec[i][j]

    # 统计纵向
    vertical = [0] * m
    for j in range(m):
        for i in range(n):
            vertical[j] += vec[i][j]

    result = float('inf')
    horizontalCut = 0
    for i in range(n):
        horizontalCut += horizontal[i]
        result = min(result, abs(sum - 2 * horizontalCut))

    verticalCut = 0
    for j in range(m):
        verticalCut += vertical[j]
        result = min(result, abs(sum - 2 * verticalCut))

    print(result)

if __name__ == "__main__":
    main()

```

<font style="color:rgb(44, 62, 80);">时间复杂度： O(n</font><sup><font style="color:rgb(44, 62, 80);">2</font></sup><font style="color:rgb(44, 62, 80);">)</font>

