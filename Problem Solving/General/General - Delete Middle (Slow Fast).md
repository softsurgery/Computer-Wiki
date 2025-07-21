```python
class ListNode(object):
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution(object):
    def deleteMiddle(self, head):
        if head is None or head.next is None:
            return None
        
        slow = head
        fast = head
        prev = None

        while fast and fast.next:
            fast = fast.next.next
            prev = slow
            slow = slow.next
        
        # Remove the middle node
        prev.next = slow.next

        return head
        
```