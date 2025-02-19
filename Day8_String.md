# ✔️344.反转字符串
> 题目链接：[344. 反转字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string/description/)
>
> 文章讲解：[代码随想录](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html)
>

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

【解法一】双指针

和 206.反转链表方法类似，也是用双指针，但更为简单

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s)-1
        while left < right:
            temp = s[left]
            s[left] = s[right]
            s[right] = temp
            left += 1
            right -= 1
```

时间复杂度：O(N)

空间复杂度：O(1)

【解法二】栈

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        stack = []
        for char in s:
            stack.append(char)
        for i in range(len(s)):
            s[i] = stack.pop()
```

时间复杂度：O(N)

空间复杂度：O(N)

# 541.反转字符串 II
题目链接：[541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/description/)

讲解文章：[代码随想录](https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html)

**<font style="color:rgb(44, 62, 80);">当需要固定规律一段一段去处理字符串的时候，要想想在for循环的表达式上做做文章。</font>**

【解法一】

** **将字符串转换成 字符列表（list），这样可以对字符串进行 就地修改，因为 Python 的字符串是 不可**变 的  **

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        i = 0
        chars = list(s)
        
        while i < len(chars):
            chars[i:i + k] = chars[i:i + k][::-1] # 反转后，更改原值为反转后值
            i += k * 2

        return ''.join(chars) # 讲字符列表转换回字符串
```

【解法二】

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        """
        1. 使用range(start, end, step)来确定需要调换的初始位置
        2. 对于字符串s = 'abc'，如果使用s[0:999] ===> 'abc'。字符串末尾如果超过最大长度，则会返回至字符串最后一个值，这个特性可以避免一些边界条件的处理。
        3. 用切片整体替换，而不是一个个替换.
        """
        def reverse_substring(text):
            left, right = 0, len(text) - 1
            while left < right:
                text[left], text[right] = text[right], text[left]
                left += 1
                right -= 1
            return text
        
        res = list(s)

        for cur in range(0, len(s), 2 * k):
            res[cur: cur + k] = reverse_substring(res[cur: cur + k])
        
        return ''.join(res)
```

# 54.替换数字
题目链接：[54. 替换数字（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1064)

讲解文章：[替换数字 | 代码随想录](https://programmercarl.com/kamacoder/0054.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html)

<font style="color:rgb(44, 62, 80);">给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。</font>

<font style="color:rgb(44, 62, 80);">思路：</font>

**<font style="color:rgb(44, 62, 80);">很多数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作。</font>**

<font style="color:rgb(44, 62, 80);">这么做有两个好处：</font>

1. <font style="color:rgb(44, 62, 80);">不用申请新数组。</font>
2. <font style="color:rgb(44, 62, 80);">从后向前填充元素，避免了从前向后填充元素时，每次添加元素都要将添加元素之后的所有元素向后移动的问题。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/49293158/1739971247916-5d4e28e0-f03f-41f8-bfcd-d23531f6a76f.png)

```cpp
#include <iostream>
using namespace std;
int main() {
    string s;
    while (cin >> s) {
        int sOldIndex = s.size() - 1;
        int count = 0; // 统计数字的个数
        for (int i = 0; i < s.size(); i++) {
            if (s[i] >= '0' && s[i] <= '9') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是将每个数字替换成"number"之后的大小
        s.resize(s.size() + count * 5);
        int sNewIndex = s.size() - 1;
        // 从后往前将数字替换为"number"
        while (sOldIndex >= 0) {
            if (s[sOldIndex] >= '0' && s[sOldIndex] <= '9') {
                s[sNewIndex--] = 'r';
                s[sNewIndex--] = 'e';
                s[sNewIndex--] = 'b';
                s[sNewIndex--] = 'm';
                s[sNewIndex--] = 'u';
                s[sNewIndex--] = 'n';
            } else {
                s[sNewIndex--] = s[sOldIndex];
            }
            sOldIndex--;
        }
        cout << s << endl;       
    }
}
```

