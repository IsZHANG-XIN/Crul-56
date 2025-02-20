# 151.反转字符串里的单词
> 题目链接：[151. 反转字符串中的单词 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-words-in-a-string/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
>

给定一个字符串，逐个翻转字符串中的每个单词。输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个

思路：

1. 先移除多余的空格⭐
2. 讲输入字符串整体反转
3. 将每个单词反转

移除空格的思路：（<u>代码不好写，注意如果 slow！=0 时需要先++</u>）

使用快慢指针，快指针遍历字符串，慢指针指向快指针元素需要去到的地方

```cpp
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0;   //整体思想参考https://programmercarl.com/0027.移除元素.html
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};
```

时间复杂度：O(N)

空间复杂度：O(1)

python：

```python
class Solution:
    def single_reverse(self, s, start: int, end: int):
        while start < end:
            s[start], s[end] = s[end], s[start]
            start += 1
            end -= 1

    def reverseWords(self, s: str) -> str:
        result = ""
        fast = 0
        # 1. 首先将原字符串反转并且除掉空格, 并且加入到新的字符串当中
        # 由于Python字符串的不可变性，因此只能转换为列表进行处理
        s = list(s)
        s.reverse()
        while fast < len(s):
            if s[fast] != " ":
                if len(result) != 0:
                    result += " "
                while s[fast] != " " and fast < len(s):
                    result += s[fast]
                    fast += 1
            else:
                fast += 1
        # 2.其次将每个单词进行翻转操作
        slow = 0
        fast = 0
        result = list(result)
        while fast <= len(result):
            if fast == len(result) or result[fast] == " ":
                self.single_reverse(result, slow, fast - 1)
                slow = fast + 1
                fast += 1
            else:
                fast += 1

        return "".join(result)
```

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 反转整个字符串
        s = s[::-1]
        # 将字符串拆分为单词，并反转每个单词
        # split()函数能够自动忽略多余的空白字符
        s = ' '.join(word[::-1] for word in s.split())
        return s
```

时间复杂度：O(N)

空间复杂度：O(N)

<font style="color:rgb(44, 62, 80);"></font>**<font style="color:rgb(44, 62, 80);">因为字符串是不可变类型，所以反转单词的时候，需要将其转换成列表，然后通过join函数再将其转换成列表，所以空间复杂度不是O(1)</font>**

# 卡：55.右旋转字符串
> 题目链接：[55. 右旋字符串（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1065)
>
> 讲解文章：[右旋字符串 | 代码随想录](https://programmercarl.com/kamacoder/0055.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)
>

<font style="color:rgb(44, 62, 80);">思路：类似上题，整体反转+局部反转</font>

```python
#获取输入的数字k和字符串
k = int(input())
s = input()

#通过切片反转第一段和第二段字符串
#注意：python中字符串是不可变的，所以也需要额外空间
s = s[len(s)-k:] + s[:len(s)-k]
print(s)
```

时间复杂度：O(N)

空间复杂度：O(N)

# 28.实现 strStr()
> 题目链接：[28. 找出字符串中第一个匹配项的下标 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)
>
> 文章讲解：[代码随想录](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
>

<font style="color:rgb(44, 62, 80);">给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。</font>

<u><font style="color:rgb(44, 62, 80);">在一个串中查找是否出现过另一个串，这是KMP的看家本领</font></u>

<font style="color:rgb(44, 62, 80);"></font>

# 459.重复的子字符串


