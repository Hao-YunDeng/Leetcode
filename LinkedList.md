### 206. Reverse Linked List
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        } 
//递归        
//         ListNode newHead = reverseList(head.next);
//         head.next.next = head;
//         head.next = null;
        
//         return newHead;

        //循环        
        // ListNode prev = null;
        // ListNode curr = head;
        // ListNode next = head;
        // while (curr != null) {
        //     next = curr.next;
        //     curr.next = prev;                        
        //     prev = curr; 
        //     curr = next;
        // }
        // return prev;
        
        //头插法
        ListNode newHead = new ListNode(-1);
        while (head != null) {
            ListNode next = head.next;
            head.next = newHead.next;
            newHead.next = head;
            head = next;
        }
        return newHead.next;
    }
}
```
### 21. Merge Two Sorted Lists
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
//法一：新建头然后逐个比较插入        
        ListNode newHead = new ListNode(-1);
        ListNode curr = newHead;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                curr.next = l1;
                l1 = l1.next;
                curr = curr.next;
            }    
            else {
                curr.next = l2;
                l2 = l2.next;
                curr = curr.next;
            }
        }
        
        if (l1 == null) {
            curr.next = l2;
        }
        else {
            curr.next = l1;
        }
        
        return newHead.next;
    }
            //法二：递归
//         if (l1 == null || l2 == null) {
//             return l1 == null ? l2 : l1;
//         }
        
//         if (l1.val < l2.val) {
//             l1.next = mergeTwoLists(l1.next, l2);
//             return l1;
//         }
//         else {
//             l2.next = mergeTwoLists(l1, l2.next);
//             return l2;
//         }
    } 
}
### 83. Remove Duplicates from Sorted List
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) return head;
        //法一：递归
        // head.next = deleteDuplicates(head.next);
        // return head.val == head.next.val ? head.next : head;
        //法二：遍历
        ListNode curr = head;
        while (curr.next != null) {
            if (curr.next.val == curr.val) {
                curr.next = curr.next.next;
            }
            else {
                //这里else是必要的，因为如果删除了并且移动的话有可能curr变null，
                //while里面curr.next就会出错
                curr = curr.next;
            }            
        }
        return head;
    }
}
```
