[W3Schools](https://www.w3schools.com/python/python_dsa_lists.asp)

In Python, lists are the built-in data structure that serves as a dynamic array.
Lists are ordered, mutable, and can contain elements of different types.

---
## Lists

A list is a built-in data structure in Python, used to store multiple elements.
Lists are used by many algorithms.

---
## Creating Lists
Lists are created using square brackets `[]`:

### Empty list 

```python
x = []  
```
  
### List with initial values  
```python
y = [1, 2, 3, 4, 5]
```  
  
### List with mixed types  
```python
z = [1, "hello", 3.14, True]
```

---

## List Methods

Python lists come with several built-in algorithms (called methods), to perform common operations like appending, sorting, and more.

Append one element to the list, and sort the list ascending:
```python
x = [9, 12, 7, 4, 11]  
```  
### Add element:  
```python
x.append(8)
```  
  
### Sort list ascending:  
```python
x.sort()
```

---

## Create Algorithms

Sometimes we want to perform actions that are not built into Python.
Then we can create our own algorithms.
For example, an algorithm can be used to find the lowest value in a list, like in the example below:

### Example
Create an algorithm to find the lowest value in a list:
```python
my_array = [7, 12, 9, 4, 11, 8]  
minVal = my_array[0]  
  
for i in my_array:  
  if i < minVal:  
    minVal = i  
  
print('Lowest value:', minVal)
```

The algorithm above is very simple, and fast enough for small data sets, but if the data is big enough, any algorithm will take time to run.
This is where optimization comes in.
Optimization is an important part of algorithm development, and of course, an important part of DSA programming.

---

## Time Complexity

When exploring algorithms, we often look at how much time an algorithm takes to run relative to the size of the data set.