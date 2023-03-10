# Day 3 LinkedList

## Problem List

#链表 #双向链表 #反转链表 #双指针

Selected by 代码随想录

1. Introduction to LinkedList
2. No. 203 https://leetcode.com/problems/remove-linked-list-elements/
3. No. 707 https://leetcode.com/problems/design-linked-list/
4. No. 206 https://leetcode.com/problems/reverse-linked-list/

## Introduction to Linked List

#### Different Linked List Type:

- Single linked list
  - One way, head: first node, tail|next  -> null

```java
public class ListNode {
	int val;
  ListNode next;
  
  public ListNode() {
  }
  
  public ListNode(int val) {
    this.val = val;
  }
  
  public ListNode(int val, ListNode next) {
    this.val = val;
    this.next = next;
  }
}
```

- Doubly linked list
  - Both way,     null <- prev |head: first node         tail |next -> null
  - Each Node of Doubly linked list has a ***prev*** attribute
- Circular linked list
  - One way, tail |next --> head

#### Some features of linked list

The way of element of linked list being stored in memory is not continuous, it depends on the memory management of specific operate system.

Delete one node of linked list meaning cut off its connection with its prev and next node, and the next node of its prev node is gonna be the next node of it.

#### **Time Complexity**

**Array** 

 $Insertion/Deletion : O(n)$  $Query : O(1)$

**Linked list**

 $Insertion/Deletion : O(1)$  $Query : O(n)$

## No. 203 Remove Linked List Elements

我的初始写法, 看了题解后发现我的没有加虚拟头结点, 也没有加pre Node

而且在写遍历到循环结束前的时候, 判定条件上出了问题,

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode cur = head;
        while (head != null && cur.val == val) {
            cur = cur.next;
            head = cur;
        }
        while (cur != null) {
            while (cur.next != null && cur.next.val == val) {
                cur.next = cur.next.next;
            }
            cur = cur.next;
        }
        return head;
    }
}
```

#### 带有虚拟头结点的写法

***虚拟头结点 --> Dummy Node***

作用:

- 实现对头结点和其他结点的统一处理

怎么做到的?

- 将dummy结点放在原本的head结点前面, 于是dummy成为了 ”虚拟头结点“

***pre结点 --> Pre Node***

作用:

- 保持对cur前一个结点的记录, 这样cur就是可以直接删除的结点
- 如果没有pre结点, 就会要把cur当作前一个结点, 删除起来思路有点绕 --> 容易出现null.next的错误

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(-1, head);
        ListNode pre = dummy;
        ListNode cur = head;
        while (cur != null) {
            if (cur.val == val) {
                pre.next = cur.next;
            } else {
                pre = cur;
            }
            cur = cur.next;
        }
        return dummy.next;
    }
}
```

## No. 707 Design Linked List

要求自己实现一个 单链表 及 双链表

值得注意的点, 一开始写总觉得很怪, 看了随想录的一点开头后, 后来才意识到:

1. ListNode类要自己单独写
2. 在初始化MyLinkedList的时候, 可以定义 ***size*** 和 ***虚拟头结点head*** 来辅助判断
   1. size要随着添加和删减变化!
   2. head始终为虚拟头结点
3. 为什么说head不是真正的头结点而是虚拟头结点呢?
   1. **因为真正的链表本身此时是空的!**
   2. 虚拟头结点帮助我们进行思考, 但是在增删改查的的时候都要注意到 ***虚拟头结点 对index的占用***
4. 先编写addAtIndex的话就不用三个方法都考虑情况了..
5. 为什么添加和删除时要先判断index >= size呢? 因为下标从0开始!

### 1. 单链表

跟着代码随想录的来分析,  来回手敲了好多遍

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) {
        this.val = val;
    }
}

class MyLinkedList {
    int size;
    ListNode head;

    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }
    
    public int get(int index) {
        if (index < 0 || index >= size){
            return -1;
        }
        ListNode cur = this.head;
        for (int i = 0; i <= index; i++) {
            cur = cur.next;
        }
        return cur.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return ;
        } 
        if (index < 0) {
            index = 0;
        }  
        size++;
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        ListNode newNode = new ListNode(val);
        newNode.next = pred.next;
        pred.next = newNode;
    }
    
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return ;
        }
        size--;
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }

}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

### 2. 双向链表

待补充

## No. 206 Reverse Linked List

自己用的是暴力解...不推荐

思路是反向存到数组, 再重新赋一边值, 虽然是O(n), 但是3N

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        int count = 0;
        ListNode cur = head;
        while (cur != null) {
            cur = cur.next;
            count++;
        }
        int[] temp = new int[count];
        cur = head;
        for(int i = count - 1; i >= 0; i--) {
            temp[i] = cur.val;
            cur = cur.next;
        }
        cur = head;
        for(int i = 0; i < count; i++) {
            cur.val = temp[i];
            cur = cur.next;
        }
        return head;
    }
}
```

### 代码随想录的好解法 ————双指针

使用pred和cur两个指针, cur在pred后面

两者渐渐向前移动, 用temp记录原本cur的后者, 把cur的next赋给pred, 把cur挪到temp



按照这个思路自己写的并不好的代码, Leetcode通过了, **但是是错误的, 因为这里head的next没有指向null !**

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode pred = head;
        ListNode cur = pred.next;
        pred.next = null;
        while (cur != null) {
            ListNode temp = cur.next;
            cur.next = pred;
            pred = cur;
            cur = temp;
        }
        return pred;
    }
}
```

代码随想录的代码, 

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        ListNode temp = null;
        while (cur != null) {
            temp = cur.next;// 保存下一个节点
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}
```

