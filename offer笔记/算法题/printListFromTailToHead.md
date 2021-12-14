## **JZ6** **从尾到头打印链表**

## 描述

输入一个链表的头节点，按链表从尾到头的顺序返回每个节点的值（用数组返回）。

如输入{1,2,3}的链表如下图:

![img](https://uploadfiles.nowcoder.com/images/20210717/557336_1626506480516/103D87B58E565E87DEFA9DD0B822C55F)

返回一个数组为[3,2,1]

0 <= 链表长度 <= 10000

栈：先进后出

ArrayList性质

ArrayList是实现了**基于动态数组**的数据结构，而LinkedList是**基于链表**的数据结构；

对于**随机访问get和set，ArrayList要优于LinkedList**，因为LinkedList要移动指针；

对于添加和删除操作add和remove，一般大家都会说LinkedList要比ArrayList快，因为ArrayList要移动数据。但是实际情况并非这样，对于添加或删除，LinkedList和ArrayList**并不能明确说明谁快谁慢**，下面会详细分析。

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
import java.util.Stack;
public class Solution {
    public ArrayList printListFromTailToHead(ListNode listNode) {
         ArrayList list = new ArrayList();

        Stack stack = new Stack();
        while (listNode != null){
            stack.push(listNode.val);
            listNode = listNode.next;
        }
        while (!stack.empty()){
            list.add(stack.pop());
        }
        return list;
        
    }
}
```

