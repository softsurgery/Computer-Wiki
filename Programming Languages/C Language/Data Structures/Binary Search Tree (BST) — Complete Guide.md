# 1. Introduction

A Binary Search Tree (BST), called **ABR (Arbre Binaire de Recherche)** in French, is a binary tree with the following property:
- Every node contains a value.
- All values in the left subtree are smaller than the node value.
- All values in the right subtree are greater than the node value.

Example:
```text
        10
       /  \
      5    15
     / \   / \
    2   7 12 20
```

Rules:
- Left child < Parent
- Right child > Parent
- 
---
# 2. BST Node Structure in C

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef struct noeud {
    int val;
    struct noeud* nl; // left child
    struct noeud* nr; // right child
} noeud;

typedef noeud* ABR;
```

---

# 3. Creating a Node

## Algorithm

1. Allocate memory.    
2. Store the value.
3. Initialize children to NULL.
4. Return the node.

## Code

```c
ABR createNode(int val) {
    ABR node = (ABR) malloc(sizeof(noeud));

    if (node == NULL) {
        printf("Memory allocation failed\n");
        exit(1);
    }

    node->val = val;
    node->nl = NULL;
    node->nr = NULL;

    return node;
}
```

---

# 4. Insertion Algorithm

## Principle

To insert a value:
- If value < current node → go left
- If value > current node → go right
- Repeat until NULL
- Insert there

## Complexity

- Average: O(log n)
- Worst case: O(n)

---
## Recursive Insertion

```c
ABR insert(ABR root, int val) {

    if (root == NULL)
        return createNode(val);

    if (val < root->val)
        root->nl = insert(root->nl, val);

    else if (val > root->val)
        root->nr = insert(root->nr, val);

    return root;
}
```

---
# 5. Searching Algorithm

## Principle

Compare target value with current node:

- Equal → found
- Smaller → search left
- Greater → search right

## Complexity
- Average: O(log n)
- Worst: O(n)

---

## Recursive Search

```c
bool search(ABR root, int val) {

    if (root == NULL)
        return false;

    if (root->val == val)
        return true;

    if (val < root->val)
        return search(root->nl, val);

    return search(root->nr, val);
}
```

---

# 6. Tree Traversals

Traversal means visiting all nodes.
There are 3 important DFS traversals.

---

# 6.1 Inorder Traversal

## Order

Left → Root → Right
## Important Property

In BST, inorder traversal prints values in sorted order.

---

## Code

```c
void inorder(ABR root) {

    if (root != NULL) {
        inorder(root->nl);
        printf("%d ", root->val);
        inorder(root->nr);
    }
}
```

---

# 6.2 Preorder Traversal

## Order

Root → Left → Right
## Uses

- Copying tree    
- Prefix expressions

---
## Code

```c
void preorder(ABR root) {

    if (root != NULL) {
        printf("%d ", root->val);
        preorder(root->nl);
        preorder(root->nr);
    }
}
```

---

# 6.3 Postorder Traversal

## Order

Left → Right → Root

## Uses

- Deleting tree    
- Expression evaluation

---

## Code

```c
void postorder(ABR root) {

    if (root != NULL) {
        postorder(root->nl);
        postorder(root->nr);
        printf("%d ", root->val);
    }
}
```

---

# 7. Counting Nodes

## Algorithm

For every node:

- Count current node = 1    
- Count left subtree
- Count right subtree

## Formula

```text
count(tree) = 1 + count(left) + count(right)
```

---

## Code

```c
int treeCount(ABR tree) {

    if (tree == NULL)
        return 0;

    return 1 + treeCount(tree->nl)
             + treeCount(tree->nr);
}
```

---

# 8. Calculating Tree Height

## Definition

Height = longest path from root to leaf.

Example:
```text
        10
       /  \
      5    15
     /
    2
```

Height = 3

---

## Formula

```text
height(tree) = 1 + max(height(left), height(right))
```

---

## Code

```c
int max(int a, int b) {
    return (a > b) ? a : b;
}

int height(ABR root) {
    if (root == NULL)
        return 0;
    return 1 + max(height(root->nl),
                   height(root->nr));
}
```

---
# 9. Finding Minimum Value

## Principle

Smallest value is always the leftmost node.

---

## Code

```c
ABR findMin(ABR root) {

    if (root == NULL)
        return NULL;

    while (root->nl != NULL)
        root = root->nl;

    return root;
}
```

---

# 10. Finding Maximum Value

## Principle

Largest value is always the rightmost node.

---

## Code

```c
ABR findMax(ABR root) {

    if (root == NULL)
        return NULL;

    while (root->nr != NULL)
        root = root->nr;

    return root;
}
```

---

# 11. Comparing Two Trees

## Principle

Two trees are equal if:
- Both are NULL
- Values are equal
- Left subtrees are equal
- Right subtrees are equal

---

## Code

```c
bool compare(ABR a1, ABR a2) {

    if (a1 == NULL && a2 == NULL)
        return true;

    if (a1 != NULL && a2 != NULL
        && a1->val == a2->val)

        return compare(a1->nl, a2->nl)
            && compare(a1->nr, a2->nr);

    return false;
}
```

---

# 12. Deleting Entire Tree

## Principle

Delete children first, then parent.
This is postorder traversal.

---

## Code

```c
void freeTree(ABR root) {

    if (root != NULL) {

        freeTree(root->nl);
        freeTree(root->nr);

        free(root);
    }
}
```

---

# 13. Deleting a Node

Deletion is the most important BST algorithm.

There are 3 cases.

---

# Case 1: Node has no children

Example:

```text
    10
   /
  5
```

Delete 5.
Just free it.

---

# Case 2: Node has one child

Example:

```text
    10
   /
  5
 /
2
```

Delete 5.
Replace 5 with child 2.

---

# Case 3: Node has two children

Example:

```text
        10
       /  \
      5    15
          / \
         12 20
```

Delete 15.
Steps:

1. Find inorder successor (smallest in right subtree)
2. Copy value
3. Delete successor

---

## Complete Deletion Code

```c
ABR deleteNode(ABR root, int val) {

    if (root == NULL)
        return NULL;

    if (val < root->val)
        root->nl = deleteNode(root->nl, val);

    else if (val > root->val)
        root->nr = deleteNode(root->nr, val);

    else {

        // Case 1 and 2
        if (root->nl == NULL) {
            ABR temp = root->nr;
            free(root);
            return temp;
        }

        else if (root->nr == NULL) {
            ABR temp = root->nl;
            free(root);
            return temp;
        }

        // Case 3
        ABR temp = findMin(root->nr);

        root->val = temp->val;

        root->nr = deleteNode(root->nr, temp->val);
    }

    return root;
}
```

---

# 14. Checking if Tree is a BST

## Principle

Every node must satisfy:

```text
min < node < max
```

---

## Code

```c
bool isBSTUtil(ABR root, int min, int max) {

    if (root == NULL)
        return true;

    if (root->val <= min || root->val >= max)
        return false;

    return isBSTUtil(root->nl, min, root->val)
        && isBSTUtil(root->nr, root->val, max);
}
```

---

# 15. Balanced vs Unbalanced Trees

## Balanced BST

Height ≈ log(n)
Fast operations.

---

## Unbalanced BST

Example:

```text
1
 \
  2
   \
    3
     \
      4
```

Behaves like linked list.

Complexity becomes O(n).

---

# 16. Time Complexity Summary

| Operation | Average  | Worst |
| --------- | -------- | ----- |
| Search    | O(log n) | O(n)  |
| Insert    | O(log n) | O(n)  |
| Delete    | O(log n) | O(n)  |
| Traversal | O(n)     | O(n)  |
| Count     | O(n)     | O(n)  |
| Height    | O(n)     | O(n)  |

---

# 17. Complete Example Program

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

typedef struct noeud {
    int val;
    struct noeud* nl;
    struct noeud* nr;
} noeud;

typedef noeud* ABR;

ABR createNode(int val) {

    ABR node = (ABR) malloc(sizeof(noeud));

    node->val = val;
    node->nl = NULL;
    node->nr = NULL;

    return node;
}

ABR insert(ABR root, int val) {

    if (root == NULL)
        return createNode(val);

    if (val < root->val)
        root->nl = insert(root->nl, val);

    else if (val > root->val)
        root->nr = insert(root->nr, val);

    return root;
}

void inorder(ABR root) {

    if (root != NULL) {
        inorder(root->nl);
        printf("%d ", root->val);
        inorder(root->nr);
    }
}

int main() {

    ABR root = NULL;

    root = insert(root, 10);
    root = insert(root, 5);
    root = insert(root, 15);
    root = insert(root, 2);
    root = insert(root, 7);
    root = insert(root, 12);
    root = insert(root, 20);

    printf("Inorder traversal: ");
    inorder(root);

    return 0;
}
```

---

# 18. Important Concepts to Remember

## BST Property

```text
left < root < right
```

---
## Inorder Traversal

Always produces sorted values.

---
## Recursive Nature

BST algorithms are naturally recursive.

---
## Deletion Complexity

Deletion is the hardest BST operation.

---
## Worst Case

A badly balanced BST becomes a linked list.

---
# 19. Advanced BST Structures

Normal BSTs can become inefficient.

Advanced self-balancing trees:
- AVL Trees    
- Red-Black Trees
- Splay Trees
- B-Trees
- Treaps

These maintain near O(log n) complexity.

---

# 20. Conclusion

Binary Search Trees are one of the most important data structures in computer science.

They allow:
- Fast search
- Fast insertion
- Fast deletion
- Sorted traversal

Understanding BSTs is essential before studying:

- AVL Trees
- Red-Black Trees
- Heaps
- Graph algorithms
- Databases
- File systems