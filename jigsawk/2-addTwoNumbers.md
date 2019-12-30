## 题干：
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

## 解法：

1. O(n) / O(n)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode ret = new ListNode(0);
        add(l1, l2, ret);
        return ret;
    }

    private void add(ListNode l1, ListNode l2, ListNode temp) {
        if (l1 == null && l2 == null)
            return;
        temp.val = l1 != null ? temp.val + l1.val : temp.val;
        temp.val = l2 != null ? temp.val + l2.val : temp.val;
        if (temp.val >= 10){
            temp.val %= 10;
            temp.next = new ListNode(1);
        }else {
            if ((l1 == null || l1.next == null) && (l2 == null || l2.next == null))
                return;
            temp.next = new ListNode(0);
        }
        add(l1 == null ? null : l1.next, l2 == null ? null : l2.next, temp.next);
    }
}

```

第一版解法，写得比较丑陋，其中包含了大量的判空操作。主要思路就是递归这个链表，每调用一遍之后移动头指针。

2. O(n) / O(n)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    	ListNode head = new ListNode(0);
        ListNode curr = head;
        int temp = 0;
        while (l1 != null || l2 != null) {
            int p = l1 != null ? l1.val : 0;
            int q = l2 != null ? l2.val : 0;
            if (p + q + temp>= 10){
                curr.next = new ListNode((p + q + temp) % 10);
                temp = 1;
            }else {
                curr.next = new ListNode((p + q + temp));
                temp = 0;
            }
            l1 = l1 != null ? l1.next : null;
            l2 = l2 != null ? l2.next : null;
            curr = curr.next;
        }
        if (temp == 1) {
            curr.next = new ListNode(1);
        }
        return head.next;
    }
}
```
循环解法，每循环一次将当前指针向后移动一位，最后注意写入需要进位的值。
