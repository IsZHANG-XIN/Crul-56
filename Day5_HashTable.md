# 哈希表基础
哈希表：依靠键（key）来访问值（value）

哈希表擅长解决的问题：**快速判断一个元素是否出现在集合里  O(1)**

哈希表的弊端：**牺牲空间换取时间**

哈希函数 Hash Function：将内容映射到索引

哈希碰撞：映射到了同一个位置

哈希碰撞的两种常见解决方法：

1. 拉链法

映射到某一个位置的元素被存储在该位置的链表中

注意❗：拉链法需要选择恰当的哈希表大小，既不要因为数组空值太多而浪费大量内存，也不要因为链表太长而造成大量的查找时间 

2. 线性探测法

要求：哈希表的 tablesize 大于 datasize，因为所有的元素都是需要存入到这个哈希表中的

常见的三种哈希结构：

1. 数组
2. set（集合）

| <font style="color:rgb(44, 62, 80);">集合</font> | <font style="color:rgb(44, 62, 80);">底层实现</font> | <font style="color:rgb(44, 62, 80);">是否有序</font> | <font style="color:rgb(44, 62, 80);">数值是否可以重复</font> | <font style="color:rgb(44, 62, 80);">能否更改数值</font> | <font style="color:rgb(44, 62, 80);">查询效率</font> | <font style="color:rgb(44, 62, 80);">增删效率</font> |
| --- | --- | --- | --- | --- | --- | --- |
| <font style="color:rgb(44, 62, 80);">set</font> | <font style="color:rgb(44, 62, 80);">红黑树</font> | <font style="color:rgb(44, 62, 80);">有序</font> | <font style="color:rgb(44, 62, 80);">否</font> | <font style="color:rgb(44, 62, 80);">否</font> | <font style="color:rgb(44, 62, 80);">O(log n)</font> | <font style="color:rgb(44, 62, 80);">O(log n)</font> |
| <font style="color:rgb(44, 62, 80);">multiset</font> | <font style="color:rgb(44, 62, 80);">红黑树</font> | <font style="color:rgb(44, 62, 80);">有序</font> | <font style="color:rgb(44, 62, 80);">是</font> | <font style="color:rgb(44, 62, 80);">否</font> | <font style="color:rgb(44, 62, 80);">O(logn)</font> | <font style="color:rgb(44, 62, 80);">O(logn)</font> |
| 👍<font style="color:rgb(44, 62, 80);">unordered_set</font> | <font style="color:rgb(44, 62, 80);">哈希表</font> | <font style="color:rgb(44, 62, 80);">无序</font> | <font style="color:rgb(44, 62, 80);">否</font> | <font style="color:rgb(44, 62, 80);">否</font> | <font style="color:rgb(44, 62, 80);">O(1)</font> | <font style="color:rgb(44, 62, 80);">O(1)</font> |


3. map（映射）

| <font style="color:rgb(44, 62, 80);">映射</font> | <font style="color:rgb(44, 62, 80);">底层实现</font> | <font style="color:rgb(44, 62, 80);">是否有序</font> | <font style="color:rgb(44, 62, 80);">数值是否可以重复</font> | <font style="color:rgb(44, 62, 80);">能否更改数值</font> | <font style="color:rgb(44, 62, 80);">查询效率</font> | <font style="color:rgb(44, 62, 80);">增删效率</font> |
| --- | --- | --- | --- | --- | --- | --- |
| <font style="color:rgb(44, 62, 80);">map</font> | <font style="color:rgb(44, 62, 80);">红黑树</font> | <font style="color:rgb(44, 62, 80);">key有序</font> | <font style="color:rgb(44, 62, 80);">key不可重复</font> | <font style="color:rgb(44, 62, 80);">key不可修改</font> | <font style="color:rgb(44, 62, 80);">O(logn)</font> | <font style="color:rgb(44, 62, 80);">O(logn)</font> |
| <font style="color:rgb(44, 62, 80);">multimap</font> | <font style="color:rgb(44, 62, 80);">红黑树</font> | <font style="color:rgb(44, 62, 80);">key有序</font> | <font style="color:rgb(44, 62, 80);">key可重复</font> | <font style="color:rgb(44, 62, 80);">key不可修改</font> | <font style="color:rgb(44, 62, 80);">O(log n)</font> | <font style="color:rgb(44, 62, 80);">O(log n)</font> |
| <font style="color:rgb(44, 62, 80);">unordered_map</font> | <font style="color:rgb(44, 62, 80);">哈希表</font> | <font style="color:rgb(44, 62, 80);">key无序</font> | <font style="color:rgb(44, 62, 80);">key不可重复</font> | <font style="color:rgb(44, 62, 80);">key不可修改</font> | <font style="color:rgb(44, 62, 80);">O(1)</font> | <font style="color:rgb(44, 62, 80);">O(1)</font> |


# 242.有效的字母异位词
> 题目链接：[242. 有效的字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-anagram/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
>

 给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。 

## 暴力解
步骤：

1. **比较两个字符串的长度**：如果两个字符串的长度不同，它们肯定不可能是字母异位词。
2. **逐个检查字符是否匹配**：从一个字符串中逐个取出字符，去另一个字符串中查找是否有相同的字符。如果找到相同的字符，就将该字符从第二个字符串中删除。最终，如果所有字符都能匹配且删除干净，那么这两个字符串是字母异位词。

```python
def isAnagram(s, t):
    if len(s) != len(t):  # 长度不同，不可能是字母异位词
        return False
    
    # 将 t 转换为列表，因为字符串是不可修改的
    t_list = list(t)
    
    # 遍历字符串 s 中的每个字符
    for char in s:
        if char in t_list:
            t_list.remove(char)  # 删除找到的字符
        else:
            return False  # 如果字符在 t 中找不到，返回 False
    
    # 如果 t 中所有字符都成功删除，t_list 应该为空
    return len(t_list) == 0

```

时间复杂度：O(n<sup>2</sup>)

空间复杂度：O(n)   字符串 `t` 转换为列表  

## 利用哈希表
思路：开辟一个数组记录 26 个字母在第一个字符串中出现的次数，然后遍历第二个字符串，将记录数组内的值对应-1，最后检查这个记录数组即可，如果有非 0，则说明要么是第一个字符串 or 第二个字符串多了元素或少了元素

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        record = [0] * 26
        for i in s:
            #并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[ord(i) - ord("a")] += 1
        for i in t:
            record[ord(i) - ord("a")] -= 1
        for i in range(26):
            if record[i] != 0:
                #record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return False
        return True
```

时间复杂度：O(n<sup></sup>)

空间复杂度：O(1) <font style="color:rgb(44, 62, 80);">常量大小的辅助数组</font>

# <font style="color:rgb(44, 62, 80);">349.两个数组的交集</font>
题目链接：[349. 两个数组的交集 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

讲解文章：[代码随想录](https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

c++用 unordered_set:（自动去重特性）

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果，之所以用set是为了给结果集去重
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            // 发现nums2的元素 在nums_set里又出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```

+ <font style="color:rgb(44, 62, 80);">时间复杂度: O(n + m) m 是最后要把 set转成vector</font>
+ <font style="color:rgb(44, 62, 80);">空间复杂度: O(n)</font>

python：

使用字典和集合：

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
    # 使用哈希表存储一个数组中的所有元素
        table = {}
        for num in nums1:
            table[num] = table.get(num, 0) + 1
        
        # 使用集合存储结果
        res = set()
        for num in nums2:
            if num in table:
                res.add(num)
                del table[num]
        
        return list(res)
```

使用数组：

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        count1 = [0]*1001
        count2 = [0]*1001
        result = []
        for i in range(len(nums1)):
            count1[nums1[i]]+=1
        for j in range(len(nums2)):
            count2[nums2[j]]+=1
        for k in range(1001):
            if count1[k]*count2[k]>0:
                result.append(k)
        return result
```

使用集合：

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

# 202.快乐数
判断一个数 n 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。

如果 n 是快乐数就返回 True ；不是，则返回 False 。

<font style="color:rgb(44, 62, 80);">使用哈希法，来判断这个sum是否重复出现，如果重复了就是return false， 否则一直找到sum为1为止。</font>

<font style="color:rgb(44, 62, 80);">判断sum是否重复出现就可以使用unordered_set。</font>

```cpp
def isHappy(self, n: int) -> bool:
    record = set()

    while True:
        n = self.get_sum(n)  # 计算每一位数字的平方和
        if n == 1:
            return True  # 如果计算结果为 1，说明是快乐数，返回 True

        # 如果计算结果重复出现，说明进入了死循环，返回 False
        if n in record:
            return False
        else:
            record.add(n)  # 将当前的 n 记录到 set 中

```

 时间复杂度：O(logn)

 空间复杂度：O(logn)

# 1.两数之和
> 题目链接：[1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/submissions/600340511/)
>
> 文章链接：[代码随想录](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)
>



```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        #创建一个集合来存储我们目前看到的数字
        seen = set()             
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [nums.index(complement), i]
            seen.add(num)
```

