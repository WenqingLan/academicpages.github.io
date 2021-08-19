---
title: 'Daily Coding Practice'
date: 2021-06-06
permalink: /posts/2021/06/coding-notes/
tags:
  - coding-notes
---


# Daily practices

## Data Structures

### LinkedList

1 -  [#24 Swap Pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

Solution 1: Recursion. Time: O(n). Space: O(n) stack space.
```java
public ListNode swapPairs(ListNode head) {
    if(head == null || head.next == null){
        return head;
    }
    ListNode next = head.next;
    head.next = swapPairs(next.next);
    next.next = head;
    return next;
}
```
Solution 2: Dummy Head for case when we need the *prev* pointer. Time: O(n). Space: O(1).

```java
public ListNode swapPairs(ListNode head) {
    ListNode dummyHead = new ListNode(0);
    dummyHead.next = head;
    ListNode temp = dummyHead;
    while (temp.next != null && temp.next.next != null) {
        ListNode node1 = temp.next;
        ListNode node2 = temp.next.next;
        temp.next = node2;
        node1.next = node2.next;
        node2.next = node1;
        temp = node1;
    }
    return dummyHead.next;
}
```

2 - [#142 Detect Cycle II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```java
  public ListNode detectCycle(ListNode head) {
      if (head == null) return null;
    
      ListNode slow = head;
      ListNode fast = head;
      while(fast != null && fast.next != null) {
          slow = slow.next;
          fast = fast.next.next;
          if (slow.equals(fast)) {
              ListNode ptr = head;
              while (ptr != slow){
                  ptr = ptr.next;
                  slow = slow.next;
              }
              return ptr;
          }
      }
      return null;
      
  }
```

3 - [#206 Reverse LinkedList](https://leetcode-cn.com/problems/reverse-linked-list/)

Flip one pointer at a time.
```java
public ListNode reverseList(ListNode head) {
    ListNode curr = head;
    ListNode prev = null;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    head = prev;
    return head;
}
```

4 - [#25 Reverse Nodes in k-Group](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)


```java
public ListNode reverseKGroup(ListNode head, int k) {
    if (k == 1) {
        return head;
    }
    ListNode curr = head;
    int size = 0;
    while (curr != null) {
        size++;
        curr = curr.next;
    }
    int iteNum = size / k;
    curr = head;
    ListNode prevLast = null; // last node of prev reversed sub list
    while (iteNum > 0) {
        int kNum = k;
        ListNode first = null;  // first node of the reversed sub list
        ListNode last = curr;
        
        ListNode prev = null;
        while (kNum > 0) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
            kNum--;
        }
        first = prev;
        last.next = curr;
        if (prevLast != null){
            prevLast.next = first; 
        }
        prevLast = last;
        
        if (iteNum == size/k) { // mark head
            head = first;
        }
        iteNum--;
    }
    
    return head;
}
```

### Stack and Queue
