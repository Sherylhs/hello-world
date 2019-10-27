## Reverse Linked List

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
    public ListNode reverseList(ListNode head) {
        if(head ==null || head.next ==null)
            return head;
        
        // null->1->2->3->4->5->null
        //  p    c
        ListNode prev = null;
        ListNode curr = head;

        while(curr !=null){
            // null->1->2->3->4->5->null
            //  p    c  n
            ListNode next = curr.next;
            
            // null<-1  2->3->4->5->null
            //  p    c  n
            curr.next = prev;
            
            // null<-1  2->3->4->5->null
            //       c  n
            //       p
            prev = curr; 
            
            // null<-1  2->3->4->5->null
            //       p  n
            //          c
            curr = next;

        }
        // null<-1<-2<-3<-4<-5  null
        //                   p  n
        //                      c
      
        return prev;
    }
}
```
