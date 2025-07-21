```c
typedef struct Node{
    int val;
    int min;
    struct Node* next
} Node;

typedef struct MinStack{
    Node* data;
} MinStack;

MinStack* minStackCreate() {
    MinStack* stack = malloc(sizeof(MinStack));
    stack->data = NULL;
    return stack;
}

void minStackPush(MinStack* obj, int val) {
    Node* node = malloc(sizeof(Node));
    node->val = val;
    node->next = obj->data;
    if (obj->data == NULL) node->min = val;
    else if (obj->data->min > val) node->min = val;
    else node->min = obj->data->min;
    
    obj->data = node;
}

void minStackPop(MinStack* obj) {
    Node* temp = obj->data;
    obj->data = obj->data->next;
    free(temp);
}

int minStackTop(MinStack* obj) {
    return obj->data->val;
}

int minStackGetMin(MinStack* obj) {
    return obj->data->min;
}

void minStackFree(MinStack* obj) {
    free(obj);
}

/**
 * Your MinStack struct will be instantiated and called as such:
 * MinStack* obj = minStackCreate();
 * minStackPush(obj, val);
 
 * minStackPop(obj);
 
 * int param_3 = minStackTop(obj);
 
 * int param_4 = minStackGetMin(obj);
 
 * minStackFree(obj);
*/
```