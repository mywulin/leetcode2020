# 栈和队列Stack和Queue

<!-- GFM-TOC -->
* [1. 最小栈](#1-最小栈-MinStack)
* [2. 有效的括号](#2-有效的括号)
* [3. ](#3-爬楼梯)
* [4.)](#4-)
<!-- GFM-TOC -->



# 1. 最小栈MinStack
155\. 最小栈

[Leetcode](https://leetcode.com/problems/min-stack/)/[力扣](https://leetcode-cn.com/problems/min-stack/)/

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

```html
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

用2个stack来存，stack1保存所有元素，minStack只保留stack1中当前的最小元素，二者长度一致

```java
public static void main(String[] args) {
        L155_MinStack obj = new L155_MinStack();
        obj.push(99);
        obj.pop();
        int param_3 = obj.top();
        int param_4 = obj.getMin();
    }

    Stack<Integer> stack = new Stack<>();
    Stack<Integer> minStack = new Stack<>();
    int min;

    public MinStack() {

    }

    public void push(int x) {
        stack.push(x);
        if (stack.size() == 0) {
            min = x;
        } else {
            min = Math.min(x, min);
        }
        minStack.push(min);
    }

    public void pop() {
        stack.pop();
        minStack.pop();
        min = minStack.peek();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
```

# 2. 有效的括号
20\. 有效的括号

[Leetcode](https://leetcode.com/problems/valid-parentheses/description/)/[力扣](https://leetcode-cn.com/problems/valid-parentheses/description/)/

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

```html
输入: "()"
输出: true
```
用stack来解，还有一个处理技巧就是，当遇到左括号时，就把对应的右括号入栈，这样的话，等遇到右括号时，只要比较pop()的括号是否跟当前括号相等即可。

```java
public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '(') {
                stack.push(')');
            } else if (c == '[') {
                stack.push(']');
            } else if (c == '{') {
                stack.push('}');
            } else if (stack.isEmpty() || stack.pop() != c) {
                return false;
            }
        }
        return stack.isEmpty();
    }
```