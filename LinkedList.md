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
