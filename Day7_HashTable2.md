# 454.四数相加
> 题目链接：[454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html#%E6%80%9D%E8%B7%AF)
>

<font style="color:rgb(38, 38, 38);">给你四个整数数组 </font>`<font style="color:rgba(38, 38, 38, 0.75);">nums1</font>`<font style="color:rgb(38, 38, 38);">、</font>`<font style="color:rgba(38, 38, 38, 0.75);">nums2</font>`<font style="color:rgb(38, 38, 38);">、</font>`<font style="color:rgba(38, 38, 38, 0.75);">nums3</font>`<font style="color:rgb(38, 38, 38);"> 和 </font>`<font style="color:rgba(38, 38, 38, 0.75);">nums4</font>`<font style="color:rgb(38, 38, 38);"> ，数组长度都是 </font>`<font style="color:rgba(38, 38, 38, 0.75);">n</font>`<font style="color:rgb(38, 38, 38);"> ，请你计算有多少个元组 </font>`<font style="color:rgba(38, 38, 38, 0.75);">(i, j, k, l)</font>`<font style="color:rgb(38, 38, 38);"> 能满足：</font>

+ `<font style="color:rgba(38, 38, 38, 0.75);">0 <= i, j, k, l < n</font>`
+ `<font style="color:rgba(38, 38, 38, 0.75);">nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0</font>`

思路：

先遍历计算 nums1+nums2 的和，放入 map or 字典 ，用 key 记录和，用 value 记录出现的次数，然后再遍历计算 nums3+nums4 的和，查看是否 =0-（nums1+nums2），是的话则 count+value。

```python
class Solution(object):
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        hashmap = dict()
        for i in nums1:
            for j in nums2:
                if i + j in hashmap:
                    hashmap[i+j] += 1
                else:
                    hashmap[i+j] = 1
        
        count = 0
        for k in nums3:
            for p in nums4:
                diff = -k-p
                if diff in hashmap:
                    count += hashmap[diff]

        return count
```

时间复杂度: O(n<sup>2</sup>)

空间复杂度: O(n<sup>2</sup>)，最坏情况下A和B的值各不相同，相加产生的数字个数为 n<sup>2</sup>

# 383.赎金信
题目链接：[383. 赎金信 - 力扣（LeetCode）](https://leetcode.cn/problems/ransom-note/description/)

讲解文章：[代码随想录](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)

<font style="color:rgb(38, 38, 38);">给你两个字符串：</font>`<font style="color:rgba(38, 38, 38, 0.75);">ransomNote</font>`<font style="color:rgb(38, 38, 38);"> 和 </font>`<font style="color:rgba(38, 38, 38, 0.75);">magazine</font>`<font style="color:rgb(38, 38, 38);"> ，判断 </font>`<font style="color:rgba(38, 38, 38, 0.75);">ransomNote</font>`<font style="color:rgb(38, 38, 38);"> 能不能由 </font>`<font style="color:rgba(38, 38, 38, 0.75);">magazine</font>`<font style="color:rgb(38, 38, 38);"> 里面的字符构成。</font>

<font style="color:rgb(38, 38, 38);">如果可以，返回</font><font style="color:rgb(38, 38, 38);"> </font>`<font style="color:rgba(38, 38, 38, 0.75);">true</font>`<font style="color:rgb(38, 38, 38);"> </font><font style="color:rgb(38, 38, 38);">；否则返回</font><font style="color:rgb(38, 38, 38);"> </font>`<font style="color:rgba(38, 38, 38, 0.75);">false</font>`<font style="color:rgb(38, 38, 38);"> </font><font style="color:rgb(38, 38, 38);">。</font>

`<font style="color:rgba(38, 38, 38, 0.75);">magazine</font>`<font style="color:rgb(38, 38, 38);"> 中的每个字符只能在 </font>`<font style="color:rgba(38, 38, 38, 0.75);">ransomNote</font>`<font style="color:rgb(38, 38, 38);"> 中使用一次。</font>

<font style="color:#0C68CA;">本题 和 242.有效的字母异位词 是一个思路 ，算是拓展题 ，在哈希法中有一些场景就是为数组量身定做的</font>

听完视频自己写的错误：

```plain
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        list1 = list(ransomNote)
        list2 = list(magazine) 不需要将字符串传为列表，因为字符串本身就是可迭代的
        hashmap = [0] * 26

        for i in list1:
            hashmap[i] += 1 不应该用i来索引对应的哈希表位置
        
        for j in list2:
            hashmap[j] -= 1

        for _ in hashmap:
            if _ != 0 :
                return false
        
        return true
```

正确：

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:

        hashmap = [0] * 26

for char in ransomNote:
    hashmap[ord(char)-ord('a')] += 1

for char in magazine:
    hashmap[ord(char)-ord('a')] -= 1

for count in hashmap:
    if count > 0 :
        return False

return True
```

❗注意：只有当 hashmap 中是大于 0 时才是失败，因为是用 magazine 中的字母组成 ransomNote，如果ransomNote 比magazine 中的字母少是可以的，也就是如果出现 hashmap 中的值小于 0 的也是可以的，注意理解题意

时间复杂度：O(N)

空间复杂度：O(1)

# 💥15.三数之和
> 题目链接：[15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/)
>
> 文章讲解：[代码随想录](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)
>

**其实这道题目使用哈希法并不十分合适**，因为在去重的操作中有很多细节需要注意，在面试中很难直接写出没有bug的代码。**这道题目使用双指针法 要比哈希法高效一些。**

**思路：**设置 i 指针依次遍历数组，再将剩余数组设置在 left 和 right 之间，通过相加>0 或<0 去缩减 sum 得到符合条件的下标进行返回。

前提是需要先对数组元素进行排序，这时没有要求返回下标时才能使用这种双指针法的原因，而两数之和那道题不行是因为要返回的是下标，一旦排序，下标就乱了。

本题的易错点在于去重！主要考虑三个数的去重。 a, b ,c, 对应的就是 nums[i]，nums[left]，nums[right]

a 如果重复了怎么办，a是nums里遍历的元素，那么应该直接跳过去。

但这里有一个问题，是判断 nums[i] 与 nums[i + 1]是否相同，还是判断 nums[i] 与 nums[i-1] 是否相同。

nums[i] 与 nums[i + 1]相同是三元组内的元素相同，如极端情况 [0,0,0]，是允许的

而 nums[i] 与 nums[i-1] 如果相同，则是应该直接跳过这次遍历，因为如果相同的话，遍历一圈得到一个重读的答案，是不要的。

<font style="color:rgb(44, 62, 80);"></font>

用例不通过：

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        fast = 1
        slow = 0
        length = len(nums)
        result = set()
        
        for i in range(length-1):
            fast += 1
            slow += 1
            for p in range(length-fast-1):
                if nums[fast]+nums[slow]+nums[p] == 0:
                    result.add((fast, slow, p))

        result = list(result)
        return result

```

正确：

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        
        for i in range(len(nums)):
            # 如果第一个元素已经大于0，不需要进一步检查
            if nums[i] > 0:
                return result
            
            # 跳过相同的元素以避免重复
            if i > 0 and nums[i] == nums[i - 1]:
                continue
                
            left = i + 1
            right = len(nums) - 1
            
            while right > left:
                sum_ = nums[i] + nums[left] + nums[right]
                
                if sum_ < 0:
                    left += 1
                elif sum_ > 0:
                    right -= 1
                else:
                    result.append([nums[i], nums[left], nums[right]])
                    
                    # 跳过相同的元素以避免重复
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while right > left and nums[left] == nums[left + 1]:
                        left += 1
                        
                    right -= 1
                    left += 1
                    
        return result
```

时间复杂度: O(n<sup>2</sup>)

空间复杂度: O(1)

# 9.四数之和
> 题目链接：[18. 四数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/4sum/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF)
>

<font style="color:rgb(38, 38, 38);">给你一个由 </font>`<font style="color:rgba(38, 38, 38, 0.75);">n</font>`<font style="color:rgb(38, 38, 38);"> 个整数组成的数组 </font>`<font style="color:rgba(38, 38, 38, 0.75);">nums</font>`<font style="color:rgb(38, 38, 38);"> ，和一个目标值 </font>`<font style="color:rgba(38, 38, 38, 0.75);">target</font>`<font style="color:rgb(38, 38, 38);"> 。请你找出并返回满足下述全部条件且</font>**<font style="color:rgb(38, 38, 38);">不重复</font>**<font style="color:rgb(38, 38, 38);">的四元组 </font>`<font style="color:rgba(38, 38, 38, 0.75);">[nums[a], nums[b], nums[c], nums[d]]</font>`<font style="color:rgb(38, 38, 38);"> （若两个四元组元素一一对应，则认为两个四元组重复）：</font>

+ `<font style="color:rgba(38, 38, 38, 0.75);">0 <= a, b, c, d < n</font>`
+ `<font style="color:rgba(38, 38, 38, 0.75);">a</font>`<font style="color:rgb(38, 38, 38);">、</font>`<font style="color:rgba(38, 38, 38, 0.75);">b</font>`<font style="color:rgb(38, 38, 38);">、</font>`<font style="color:rgba(38, 38, 38, 0.75);">c</font>`<font style="color:rgb(38, 38, 38);"> </font><font style="color:rgb(38, 38, 38);">和</font><font style="color:rgb(38, 38, 38);"> </font>`<font style="color:rgba(38, 38, 38, 0.75);">d</font>`<font style="color:rgb(38, 38, 38);"> </font>**<font style="color:rgb(38, 38, 38);">互不相同</font>**
+ `<font style="color:rgba(38, 38, 38, 0.75);">nums[a] + nums[b] + nums[c] + nums[d] == target</font>`

<font style="color:rgb(38, 38, 38);">你可以按 </font>**<font style="color:rgb(38, 38, 38);">任意顺序</font>**<font style="color:rgb(38, 38, 38);"> 返回答案 。</font>

<font style="color:rgb(38, 38, 38);"></font>

**思路：**和三数之和方法一样，都是使用**<u>双指针法</u>**, 基本解法就是在[15.三数之和(opens new window)](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)的基础上再套一层for循环。

但是有一些细节需要注意，例如： 不要判断`nums[k] > target` 就返回了，三数之和 可以通过 `nums[i] > 0` 就返回了，因为 0 已经是确定的数了，四数之和这道题目 target是任意值。比如：数组是`[-4, -3, -2, -1]`，`target`是`-10`，不能因为`-4 > -10`而跳过。但是我们依旧可以去做剪枝，逻辑变成`nums[i] > target && (nums[i] >=0 || target >= 0)`就可以了。

四数之和的双指针解法是两层for循环nums[k] + nums[i]为确定值，依然是循环内有left和right下标作为双指针，找出nums[k] + nums[i] + nums[left] + nums[right] == target的情况，三数之和的时间复杂度是O(n^2)，四数之和的时间复杂度是O(n^3) 。

那么一样的道理，五数之和、六数之和等等都采用这种解法。

对于[15.三数之和(opens new window)](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)双指针法就是将原本暴力O(n^3)的解法，降为O(n^2)的解法，四数之和的双指针解法就是将原本暴力O(n^4)的解法，降为O(n^3)的解法。

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        result = []
        for i in range(n):
            if nums[i] > target and nums[i] > 0 and target > 0:# 剪枝（可省）
                break
            if i > 0 and nums[i] == nums[i-1]:# 去重
                continue
            for j in range(i+1, n):
                if nums[i] + nums[j] > target and target > 0: #剪枝（可省）
                    break
                if j > i+1 and nums[j] == nums[j-1]: # 去重
                    continue
                left, right = j+1, n-1
                while left < right:
                    s = nums[i] + nums[j] + nums[left] + nums[right]
                    if s == target:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
                    elif s < target:
                        left += 1
                    else:
                        right -= 1
        return result
```

<font style="color:rgb(44, 62, 80);">时间复杂度: O(n</font><sup><font style="color:rgb(44, 62, 80);">3</font></sup><font style="color:rgb(44, 62, 80);">)</font>

<font style="color:rgb(44, 62, 80);">空间复杂度: O(1)</font>

  
 

