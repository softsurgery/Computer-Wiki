```c
struct ListNode* reverseKGroup(struct ListNode* head, int k) {
    if (head == NULL || k == 1) {
        return head;
    }
    
    // Check if there are at least k nodes remaining
    struct ListNode* curr = head;
    int count = 0;
    while (count < k && curr != NULL) {
        curr = curr->next;
        count++;
    }
    
    if (count < k) {
        return head; // Not enough nodes to reverse
    }
    
    // Reverse the first k nodes
    struct ListNode* prev = NULL;
    struct ListNode* next = NULL;
    curr = head;
    count = 0;
    
    while (count < k && curr != NULL) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
        count++;
    }
    
    // Recursively reverse the remaining list and link it
    if (next != NULL) {
        head->next = reverseKGroup(next, k);
    }
    
    return prev; // prev is the new head of the reversed portion
}
```