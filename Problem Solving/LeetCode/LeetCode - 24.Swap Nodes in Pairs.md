```python
class Solution(object):
    def swapPairs(self, head):
        if head == None or head.next == None:
            return head
        else:
            prev = None
            next = None
            curr = head
            count = 0
            while count < 2 and curr != None:
                next = curr.next
                curr.next = prev
                prev = curr
                curr = next
                count += 1
            if next != None:
                head.next = self.swapPairs(next)
            return prev
```