# 232.用栈实现队列
> 题目链接：[232. 用栈实现队列 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-queue-using-stacks/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E6%80%9D%E8%B7%AF)
>

使用栈实现队列的下列操作：

push(x) -- 将一个元素放入队列的尾部。  
pop() -- 从队列首部移除元素。  
peek() -- 返回队列首部的元素。  
empty() -- 返回队列是否为空。

**思路：**一个输入栈，一个输出栈

**key：**如果输出栈不为空的话，pop 直接出一个就行，但如果输出栈已经空了，那需要把输入栈全部都倒进来，再出栈，不然顺序会错

```cpp
class MyQueue {
public:
stack<int> stIn;
stack<int> stOut;
/** Initialize your data structure here. */
MyQueue() {

}
/** Push element x to the back of queue. */
void push(int x) {
    stIn.push(x);
}

/** Removes the element from in front of queue and returns that element. */
int pop() {
    // 只有当stOut为空的时候，再从stIn里导入数据（导入stIn全部数据）
    if (stOut.empty()) {
        // 从stIn导入数据直到stIn为空
        while(!stIn.empty()) {
            stOut.push(stIn.top());
            stIn.pop();
        }
    }
    int result = stOut.top();
    stOut.pop();
    return result;
}

/** Get the front element. */
int peek() {
    int res = this->pop(); // 直接使用已有的pop函数
    stOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
    return res;
}

/** Returns whether the queue is empty. */
bool empty() {
    return stIn.empty() && stOut.empty();
}
};

```

```python
class MyQueue:

    def __init__(self):
        """
        in主要负责push，out主要负责pop
        """
        self.stack_in = []
        self.stack_out = []


    def push(self, x: int) -> None:
        """
        有新元素进来，就往in里面push
        """
        self.stack_in.append(x)


    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        if self.empty():
            return None
        
        if self.stack_out:
            return self.stack_out.pop()
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()


    def peek(self) -> int:
        """
        Get the front element.
        """
        ans = self.pop()
        self.stack_out.append(ans)
        return ans


    def empty(self) -> bool:
        """
        只要in或者out有元素，说明队列不为空
        """
        return not (self.stack_in or self.stack_out)
```

时间复杂度: 都为O(1)。pop和peek看起来像O(n)，实际上一个循环n会被使用n次，最后还是O(1)。

空间复杂度: O(n)

# 225.用队列实现栈
> 题目链接：[225. 用队列实现栈 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-stack-using-queues/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html#%E6%80%9D%E8%B7%AF)
>

<font style="color:rgb(44, 62, 80);">使用队列实现栈的下列操作：</font>

+ <font style="color:rgb(44, 62, 80);">push(x) -- 元素 x 入栈</font>
+ <font style="color:rgb(44, 62, 80);">pop() -- 移除栈顶元素</font>
+ <font style="color:rgb(44, 62, 80);">top() -- 获取栈顶元素</font>
+ <font style="color:rgb(44, 62, 80);">empty() -- 返回栈是否为空</font>

思路一：**<font style="color:rgb(44, 62, 80);">用两个队列que1和que2实现队列的功能，que2其实完全就是一个备份的作用</font>**

![](https://cdn.nlark.com/yuque/0/2025/gif/49293158/1740145562403-5dfc8c09-efc3-41f7-94ec-c855814eda10.gif)

```cpp
class MyStack {
public:
    queue<int> que1;
    queue<int> que2; // 辅助队列，用来备份

    /** Initialize your data structure here. */
    MyStack() {

    }

    /** Push element x onto stack. */
    void push(int x) {
        que1.push(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int size = que1.size();
        size--;
        while (size--) { // 将que1 导入que2，但要留下最后一个元素
            que2.push(que1.front());
            que1.pop();
        }

        int result = que1.front(); // 留下的最后一个元素就是要返回的值
        que1.pop();
        que1 = que2;            // 再将que2赋值给que1
        while (!que2.empty()) { // 清空que2
            que2.pop();
        }
        return result;
    }

    /** Get the top element.
     ** Can not use back() direactly.
     */
    int top(){
        int size = que1.size();
        size--;
        while (size--){
            // 将que1 导入que2，但要留下最后一个元素
            que2.push(que1.front());
            que1.pop();
        }

        int result = que1.front(); // 留下的最后一个元素就是要回返的值
        que2.push(que1.front());   // 获取值后将最后一个元素也加入que2中，保持原本的结构不变
        que1.pop();

        que1 = que2; // 再将que2赋值给que1
        while (!que2.empty()){
            // 清空que2
            que2.pop();
        }
        return result;
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return que1.empty();
    }
};
```

时间复杂度: pop为O(n)，top为O(n)，其他为O(1)

空间复杂度: O(n)

优化思路：**<font style="color:rgb(44, 62, 80);">一个队列在模拟栈弹出元素的时候只要将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部，此时再去弹出元素就是栈的顺序了。</font>**

```python
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
        # 写法一：
        # if self.empty():
        #     return None
        # return self.que[-1]

        # 写法二：
        if self.empty():
            return None
        for i in range(len(self.que)-1):
            self.que.append(self.que.popleft())
        temp = self.que.popleft()
        self.que.append(temp)
        return temp

    def empty(self) -> bool:
        return not self.que
```

时间复杂度: pop为O(n)，top为O(n)，其他为O(1)

空间复杂度: O(n)

# 20.有效的括号
> 题目链接：[20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/description/)
>
> 讲解文章：[代码随想录](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E6%80%9D%E8%B7%AF)
>

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

+ 左括号必须用相同类型的右括号闭合。
+ 左括号必须以正确的顺序闭合。
+ 注意空字符串可被认为是有效字符串。

**栈的经典应用！**

key：搞清楚有几种匹配失败的情况

1. <font style="color:rgb(44, 62, 80);">第一种情况，字符串里左方向的括号多余了 ，所以不匹配。</font>
2. <font style="color:rgb(44, 62, 80);">第二种情况，括号没有多余，但是 括号的类型没有匹配上。</font>
3. <font style="color:rgb(44, 62, 80);">第三种情况，字符串里右方向的括号多余了，所以不匹配。</font>

所以：

<font style="color:rgb(44, 62, 80);">第一种情况：已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false</font>

<font style="color:rgb(44, 62, 80);">第二种情况：遍历字符串匹配的过程中，发现栈里没有要匹配的字符。所以return false</font>

<font style="color:rgb(44, 62, 80);">第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号return false</font>

trick：遇到左括号的时候入栈对应的右括号，这样只需要匹配字符串的元素和栈顶元素是否相等即可，操作简单

```cpp
class Solution {
public:
bool isValid(string s) {
    if (s.size() % 2 != 0) return false; // 如果s的长度为奇数，一定不符合要求
    stack<char> st;
    for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(') st.push(')');
        else if (s[i] == '{') st.push('}');
        else if (s[i] == '[') st.push(']');
            // 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
            // 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
        else if (st.empty() || st.top() != s[i]) return false;
        else st.pop(); // st.top() 与 s[i]相等，栈弹出元素
    }
    // 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
    return st.empty();
}
};
```

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        for item in s:
            if item == '(':
                stack.append(')')
            elif item == '[':
                stack.append(']')
            elif item == '{':
                stack.append('}')
            elif not stack or stack[-1] != item:
                return False
            else:
                stack.pop()

        return True if not stack else False
```

<font style="color:rgb(44, 62, 80);">时间复杂度: O(n)</font>

<font style="color:rgb(44, 62, 80);">空间复杂度: O(n)</font>

# <font style="color:rgb(44, 62, 80);">1047.删除字符串中的所有相邻重复项</font>
> <font style="color:rgb(44, 62, 80);">题目链接：</font>[1047. 删除字符串中的所有相邻重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)
>
> <font style="color:rgb(44, 62, 80);">讲解文章：</font>[代码随想录](https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
>

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

注意：最后从栈里弹出的元素是倒序的，所以再对字符串进行反转一下，就得到了最终的结果。

```cpp
class Solution {
public:
string removeDuplicates(string S) {
    stack<char> st;
    for (char s : S) {
        if (st.empty() || s != st.top()) {
            st.push(s);
        } else {
            st.pop(); // s 与 st.top()相等的情况
        }
    }
    string result = "";
    while (!st.empty()) { // 将栈中元素放到result字符串汇总
        result += st.top();
        st.pop();
    }
    reverse (result.begin(), result.end()); // 此时字符串需要反转一下
    return result;

}
};
```

时间复杂度: O(n)

空间复杂度: O(n)

直接拿字符串作为栈，省去出栈后还要反转下

```cpp
class Solution {
public:
    string removeDuplicates(string S) {
        string result;
        for(char s : S) {
            if(result.empty() || result.back() != s) {
                result.push_back(s);
            }
            else {
                result.pop_back();
            }
        }
        return result;
    }
};
```

时间复杂度: O(n)

空间复杂度: O(1)，返回值不计空间复杂度

```python
# 方法一，使用栈
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list()
        for item in s:
            if res and res[-1] == item:
                res.pop()
            else:
                res.append(item)
        return "".join(res)  # 字符串拼接
```

不好理解！但如果不让用栈，就这样用双指针

```python
# 方法二，使用双指针模拟栈，如果不让用栈可以作为备选方法。
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list(s)
        slow = fast = 0
        length = len(res)

        while fast < length:
            # 如果一样直接换，不一样会把后面的填在slow的位置
            res[slow] = res[fast]
            
            # 如果发现和前一个一样，就退一格指针
            if slow > 0 and res[slow] == res[slow - 1]:
                slow -= 1
            else:
                slow += 1
            fast += 1
            
        return ''.join(res[0: slow])
```

