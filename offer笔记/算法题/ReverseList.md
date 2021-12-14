## JZ24 反转链表

## 描述

给定一个单链表的头结点pHead，长度为n，反转该链表后，返回新链表的表头。

数据范围： n≤1000

要求：空间复杂度 O(1)*O*(1) ，时间复杂度 O(n)*O*(*n*) 。

如当输入链表{1,2,3}时，

经反转后，原链表变为{3,2,1}，所以对应的输出为{3,2,1}。

以上转换过程如下图所示：

![img](https://uploadfiles.nowcoder.com/images/20211014/423483716_1634206291971/4A47A0DB6E60853DEDFCFDF08A5CA249)

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
import java.util.Stack;
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode newHead = null;
        while(head != null){
            ListNode temp = head.next;
            head.next = newHead;
            newHead = head;
            head = temp;
        }      
        return newHead;
    }   
}
```

- 在遍历链表时，**将当前节点的next 指针改为指向前一个节点**。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用。

 