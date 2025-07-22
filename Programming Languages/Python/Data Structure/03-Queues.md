[W3Schools](https://www.w3schools.com/python/python_dsa_queues.asp)

A queue is a linear data structure that follows the First-In-First-Out (FIFO) principle.

---
## Queues
Think of a queue as people standing in line in a supermarket.
The first person to stand in line is also the first who can pay and leave the supermarket.

Basic operations we can do on a queue are:
- **Enqueue:** Adds a new element to the queue.
- **Dequeue:** Removes and returns the first (front) element from the queue.
- **Peek:** Returns the first element in the queue.
- **isEmpty:** Checks if the queue is empty.
- **Size:** Finds the number of elements in the queue.

Queues can be implemented by using arrays or linked lists.
Queues can be used to implement job scheduling for an office printer, order processing for e-tickets, or to create algorithms for breadth-first search in graphs.

---

## Queue Implementation using Python Lists

For Python lists (and arrays), a Queue can look and behave like this:

```python
x = [5, 6, 2, 9, 3, 8, 4, 2]
```

Since Python lists has good support for functionality needed to implement queues, we start with creating a queue and do queue operations with just a few lines:

Using a Python list as a queue:
```python
queue = []  
```  
### Enqueue  
```python
queue.append('A')  
queue.append('B')  
queue.append('C')  
print("Queue: ", queue)  
```
### Peek  
```python
frontElement = queue[0]  
print("Peek: ", frontElement)  
```
### Dequeue  
```python
poppedElement = queue.pop(0)  
print("Dequeue: ", poppedElement)  
print("Queue after Dequeue: ", queue)  
```
### isEmpty  
```python
isEmpty = not bool(queue)  
print("isEmpty: ", isEmpty)  
```
### Size  
```python
print("Size: ", len(queue))
```
**Note:** While using a list is simple, removing elements from the beginning (dequeue operation) requires shifting all remaining elements, making it less efficient for large queues.

---

## Implementing a Queue Class

Here's a complete implementation of a Queue class:

### Example

Using a Python class as a queue:
```python
class Queue:  
  def __init__(self):  
    self.queue = []  
      
  def enqueue(self, element):  
    self.queue.append(element)  
  
  def dequeue(self):  
    if self.isEmpty():  
      return "Queue is empty"  
    return self.queue.pop(0)  
  
  def peek(self):  
    if self.isEmpty():  
      return "Queue is empty"  
    return self.queue[0]  
  
  def isEmpty(self):  
    return len(self.queue) == 0  
  
  def size(self):  
    return len(self.queue)  
  
```
## Create a queue  
```python
myQueue = Queue()  
  
myQueue.enqueue('A')  
myQueue.enqueue('B')  
myQueue.enqueue('C')  
  
print("Queue: ", myQueue.queue)  
print("Peek: ", myQueue.peek())  
print("Dequeue: ", myQueue.dequeue())  
print("Queue after Dequeue: ", myQueue.queue)  
print("isEmpty: ", myQueue.isEmpty())  
print("Size: ", myQueue.size())
```

---
## Queue Implementation using Linked Lists

A linked list consists of nodes with some sort of data, and a pointer to the next node.!

A big benefit with using linked lists is that nodes are stored wherever there is free space in memory, the nodes do not have to be stored contiguously right after each other like elements are stored in arrays. Another nice thing with linked lists is that when adding or removing nodes, the rest of the nodes in the list do not have to be shifted.


This is how a queue can be implemented using a linked list.

### Example

Creating a Queue using a Linked List:
```python
class Node:  
  def __init__(self, data):  
    self.data = data  
    self.next = None  
  
class Queue:  
  def __init__(self):  
    self.front = None  
    self.rear = None  
    self.length = 0  
  
  def enqueue(self, element):  
    new_node = Node(element)  
    if self.rear is None:  
      self.front = self.rear = new_node  
      self.length += 1  
      return  
    self.rear.next = new_node  
    self.rear = new_node  
    self.length += 1  
  
  def dequeue(self):  
    if self.isEmpty():  
      return "Queue is empty"  
  
  def isEmpty(self):  
    return self.length == 0  
  
  def size(self):  
    return self.length  
  
  def printQueue(self):  
    temp = self.front  
    while temp:  
      print(temp.data, end=" ")  
      temp = temp.next  
    print()  
  
  def dequeue(self):  
    if self.isEmpty():  
      return "Queue is empty"  
    temp = self.front  
    self.front = temp.next  
    self.length -= 1  
    if self.front is None:  
      self.rear = None  
    return temp.data  
  
  def peek(self):  
    if self.isEmpty():  
      return "Queue is empty"  
    return self.front.data  
  
  def isEmpty(self):  
    return self.length == 0  
  
  def size(self):  
    return self.length  
  
  def printQueue(self):  
    temp = self.front  
    while temp:  
      print(temp.data, end=" -> ")  
      temp = temp.next  
    print()  
```
  
## Create a queue  
```python
myQueue = Queue()  
  
myQueue.enqueue('A')  
myQueue.enqueue('B')  
myQueue.enqueue('C')  
  
print("Queue: ", end="")  
myQueue.printQueue()  
print("Peek: ", myQueue.peek())  
print("Dequeue: ", myQueue.dequeue())  
print("Queue after Dequeue: ", end="")  
myQueue.printQueue()  
print("isEmpty: ", myQueue.isEmpty())  
print("Size: ", myQueue.size())
```

Reasons for using linked lists to implement queues:

- **Dynamic size:** The queue can grow and shrink dynamically, unlike with arrays.
- **No shifting:** The front element of the queue can be removed (enqueue) without having to shift other elements in the memory.

Reasons for **not** using linked lists to implement queues:

- **Extra memory:** Each queue element must contain the address to the next element (the next linked list node).
- **Readability:** The code might be harder to read and write for some because it is longer and more complex.

---
## Common Queue Applications

Queues are used in many real-world scenarios:

- Task scheduling in operating systems
- Breadth-first search in graphs
- Message queues in distributed systems