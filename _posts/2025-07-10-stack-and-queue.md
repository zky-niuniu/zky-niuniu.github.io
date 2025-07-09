# 栈和队列

## 1.基础知识

队列先进先出，一个口进，一个口出；栈先进后出，出口入口在同一端。

## 2.栈实现队列

使用栈实现队列的下列操作：

push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。

内部使用两个栈（stIn和stOut）来模拟队列的操作。

1. **两个栈的作用**：
   - `stIn`：用于接收所有入队(push)操作的元素
   - `stOut`：用于执行出队(pop)和查看队首(peek)操作
2. **关键操作**：
   - `push(x)`：直接将元素压入`stIn`栈
   - `pop()`：
     - 如果`stOut`为空，将`stIn`中的所有元素依次弹出并压入`stOut`（这会反转元素的顺序）
     - 然后从`stOut`弹出顶部元素
   - `peek()`：利用`pop()`获取元素，但要把元素重新压回`stOut`
   - `empty()`：当两个栈都为空时队列为空

## 时间复杂度分析

- `push(x)`：O(1)
- `pop()`：平均O(1)，最坏O(n)（需要转移元素时）
- `peek()`：同`pop()`
- `empty()`：O(1)

```c++
#include<iostream>
#include<stack>
using namespace std;
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
int main(){
    MyQueue queue;
    queue.push(1);
    queue.push(2);
    queue.pop();
    queue.push(3);
    queue.push(4);
    queue.pop();
    queue.pop();
    queue.pop();
    queue.empty();
}
```

## 3.队列实现栈

使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

内部使用两个队列（que1和que2）来模拟栈的操作。

1. **两个队列的作用**：

   - `que1`：主队列，存储栈中元素
   - `que2`：辅助队列，用于临时存储元素

2. **关键操作**：

   - `push(x)`：直接将元素加入`que1`（O(1)时间复杂度）
   - `pop()`：
     - 将`que1`中的前n-1个元素转移到`que2`
     - 取出并返回`que1`的最后一个元素
     - 将`que2`的内容移回`que1`
     - 清空`que2`
   - `top()`：
     - 类似于`pop()`但不移除最后一个元素
     - 需要将最后一个元素也加入`que2`以保持结构不变
   - `empty()`：检查`que1`是否为空

   1. **可以只使用一个队列实现**：
      - 在pop或top操作时，可以将队列前n-1个元素依次出队并重新入队
      - 这样就不需要第二个队列
   2. **简化top操作**：
      - 可以维护一个变量记录栈顶元素
      - 这样top操作可以达到O(1)时间复杂度

```c++
#include<iostream>
#include<queue>
#include <cassert>
using namespace std;
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
int main(){
     MyStack s;
    
    // 测试1: 基本功能测试
    s.push(1);
    s.push(2);
    s.push(3);
    assert(s.top() == 3);  // 查看栈顶
    assert(s.pop() == 3);  // 出栈
    assert(s.pop() == 2);
    s.push(4);
    assert(s.pop() == 4);
    assert(s.pop() == 1);
    assert(s.empty());     // 栈应为空
    
    // 测试2: 交替push和pop
    s.push(5);
    assert(s.top() == 5);
    s.push(6);
    assert(s.pop() == 6);
    s.push(7);
    assert(s.pop() == 7);
    assert(s.pop() == 5);
    assert(s.empty());
    
    // 测试3: 空栈测试
    assert(s.empty());
    // 对空栈执行pop或top应该抛出异常，但这里没有实现异常处理
    
    cout << "所有测试用例通过!" << endl;
    return 0;
}
```

优化：

1. **核心思想**：
   - 利用队列的FIFO特性，通过**循环转移元素**来实现栈的LIFO特性
   - 每次pop或top操作时，将队列前n-1个元素依次移到队列尾部
2. **关键操作**：
   - `push(x)`：直接入队（O(1)时间复杂度）
   - `pop()`：
     - 将前n-1个元素移到队列尾部
     - 此时队列头部元素就是栈顶元素
     - 弹出并返回该元素
   - `top()`：
     - 类似pop操作但不移除元素
     - 需要将元素重新加入队列尾部以保持结构
   - `empty()`：直接检查队列是否为空

```c++
class MyStack {
public:
    queue<int> que;

    MyStack() {

    }

    void push(int x) {
        que.push(x);
    }

    int pop() {
        int size = que.size();
        size--;
        while (size--) { // 将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部
            que.push(que.front());
            que.pop();
        }
        int result = que.front(); // 此时弹出的元素顺序就是栈的顺序了
        que.pop();
        return result;
    }

    int top(){
        int size = que.size();
        size--;
        while (size--){
            // 将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部
            que.push(que.front());
            que.pop();
        }
        int result = que.front(); // 此时获得的元素就是栈顶的元素了
        que.push(que.front());    // 将获取完的元素也重新添加到队列尾部，保证数据结构没有变化
        que.pop();
        return result;
    }

    bool empty() {
        return que.empty();
    }
};
```

## 4.有效的括号

先来分析一下 这里有三种不匹配的情况，

1. 第一种情况，字符串里左方向的括号多余了 ，所以不匹配。
2. 第二种情况，括号没有多余，但是 括号的类型没有匹配上。
3. 第三种情况，字符串里右方向的括号多余了，所以不匹配。

```c++
#include<iostream>
#include<vector>
#include<string>
using namespace std;
//第一种方法
bool isValid(string s) {
    string stack;
    for(auto c : s) {
        if(c == '(' || c == '[' || c == '{') {
            stack.push_back(c);
        }
        else {
            if(stack.empty()) 
            {
                cout<<"no";
                return false;
            }
            char top = stack.back();
            if((c == ')' && top == '(') ||
               (c == ']' && top == '[') ||
               (c == '}' && top == '{')) {
                stack.pop_back();
            }
            else {
                cout<<"no";
                return false;
            }
        }
    }
    if(stack.empty()) {
        cout << "yes";
        return true;
    }
    return 0;
}
//第二种方法
bool isValid01(string s) {
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
int main() {
    isValid("{}");
    return 0;
}
```

## 5.删除字符串中的所有相邻重复

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

示例：

- 输入："abbaca"
- 输出："ca"
- 解释：例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

这个就像是做消消乐，栈存放遍历过的元素，当遍历当前的这个元素的时候，去栈里看一下我们是不是遍历过相同数值的相邻元素。

```c++
#include<iostream>
#include<stack>
using namespace std;
string same(string s) {
    stack<char> stk;
    for (char ch : s) {
        if (stk.empty() || ch != stk.top()) {
            stk.push(ch);
        } else {
            stk.pop();
        }
    }
    string result = "";
    while (!stk.empty()) {
        result += stk.top();
        stk.pop();
    }
    reverse(result.begin(), result.end());
    return result;
}

int main() {
    string s = "abbaac";
    cout << same(s);  // 输出 "a"
    return 0;
}
```

## 6.逆波兰表达式

根据 逆波兰表示法，求表达式的值。

有效的运算符包括 + , - , * , / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：

整数除法只保留整数部分。 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

示例 1：

- 输入: ["2", "1", "+", "3", " * "]
- 输出: 9
- 解释: 该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9

示例 2：

- 输入: ["4", "13", "5", "/", "+"]
- 输出: 6
- 解释: 该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6

示例 3：

- 输入: ["10", "6", "9", "3", "+", "-11", " * ", "/", " * ", "17", "+", "5", "+"]
- 输出: 22
- 解释:该算式转化为常见的中缀算术表达式为：

这个就是遇到数字就入栈，遇到运算符就出栈，参与运算，如果栈不是空的那么得到的数字继续入栈，如果栈空就输出结果。

```c++
#include<iostream>
#include<stack>
#include<string>
#include <vector>
using namespace std;
int calculate(vector<string> tokens){
    stack<long long> st; 
        for (int i = 0; i < tokens.size(); i++) {
            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/") {
                long long num1 = st.top();
                st.pop();
                long long num2 = st.top();
                st.pop();
                if (tokens[i] == "+") st.push(num2 + num1);
                if (tokens[i] == "-") st.push(num2 - num1);
                if (tokens[i] == "*") st.push(num2 * num1);
                if (tokens[i] == "/") st.push(num2 / num1);
            } else {
                st.push(stoll(tokens[i]));
            }
        }

        long long result = st.top();
        st.pop(); // 把栈里最后一个元素弹出（其实不弹出也没事）
        return result;
}
int main(){
    vector<string> str{"2", "1", "+", "3", "*"};
    cout<<calculate(str);
}
```



## 7.滑动窗口最大值

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。返回滑动窗口中的最大值。

进阶：你能在线性时间复杂度内解决此题吗？

这是使用单调队列的经典题目。

如果我们有个

## 8.前k个高频元素

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

示例 1:

- 输入: nums = [1,1,1,2,2,3], k = 2
- 输出: [1,2]

示例 2:

- 输入: nums = [1], k = 1
- 输出: [1]

提示：

- 你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。

- 你的算法的时间复杂度必须优于
  $$
  O(n \log n)
  $$
  其中n 是数组的大小。

- 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。

- 你可以按任意顺序返回答案。

