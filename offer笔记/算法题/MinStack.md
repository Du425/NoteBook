#### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。



解题思路：
**普通栈的 push() 和 pop() 函数的复杂度为 O(1) ；而获取栈最小值 min() 函数需要遍历整个栈，复杂度为 O(N)。**

本题难点： **将 min() 函数复杂度降为 O(1)** ，可通过建立辅助栈实现；
数据栈 A： 栈 A **用于存储所有元素**，保证入栈 push() 函数、出栈 pop() 函数、获取栈顶 top() 函数的正常逻辑。
**辅助栈 B：** 栈 B 中存储栈 A 中所有 **非严格降序 的元素**，则栈 A 中的最小元素始终对应栈 B 的栈顶元素，即 min() 函数只需返回栈 BB 的栈顶元素即可。
因此，只需设法维护好 栈 B 的元素，使其保持非严格降序，即可实现 min() 函数的 O(1) 复杂度。

**Stack.Peek 与 stack.pop 的区别**

**peek()只取值 不会删掉值 .pop()取完栈顶的值还会把它删掉**

```java
class MinStack {

    Stack<Integer> A, B;

    /** initialize your data structure here. */
    public MinStack() {
        A = new Stack<>();
        B = new Stack<>();

    }
    
    public void push(int x) {
        A.add(x);
        if(B.isEmpty() || B.peek()>=x){
            B.add(x);
        }
    }
    
    public void pop() {
        if(A.pop().equals(B.peek())){
            B.pop();
        }
    }
    
    public int top() {
        return A.peek();
    }
    
    public int min() {
        return B.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

