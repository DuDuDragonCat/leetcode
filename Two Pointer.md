# Two Pointer

- Update: 20210919
- Qes: 19

### 19. Remove Nth Node From End of List

分快和慢的List，快和慢的間距為n，當快的走到終點時，慢的跳過一個即可刪除第n個元素，需注意只有一個元素的情況。

``` Python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        list_fast = head
        list_slow = head
        for _ in range(n):
            list_fast = list_fast.next
        if list_fast==None:
            return(head.next)
        while list_fast.next:
            list_fast, list_slow = list_fast.next, list_slow.next
        list_slow.next = list_slow.next.next
        return(head)
```