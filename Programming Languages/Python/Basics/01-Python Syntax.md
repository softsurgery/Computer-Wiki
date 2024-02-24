[W3School](https://www.w3schools.com/python/python_syntax.asp)

# Execute Python Syntax:

As we learned in the previous page, Python syntax can be executed by writing directly in the Command Line:

```python
>>> print("Hello, World!")
Hello, World!
```

# Python Indentation:

Indentation refers to the spaces at the beginning of a code line.

Where in other programming languages the indentation in code is for readability only, the indentation in Python is very important.

Python uses indentation to indicate a block of code.

```python
if 5 > 2:  
	print("Five is greater than two!")
```

Python will give you an error if you skip the indentation:

```python
if 5 > 2:
print("Five is greater than two!")
```

The number of spaces is up to you as a programmer, but it has to be at least one.

You have to use the same number of spaces in the same block of code, otherwise Python will give you an error.

# Python Variables:

In Python, variables are created when you assign a value to it:

```python
x = 5
y = "Hello, World!"
```

Python has no command for declaring a variable.

# Comments:

Python has commenting capability for the purpose of in-code documentation.

Comments start with a #, and Python will render the rest of the line as a comment:

```python
#This is a comment.
print("Hello, World!")
```

# input() Function:

## Example:

Ask for the user's name and print it:

```python
print('Enter your name:')
x = input()
print('Hello, ' + x)
```

## Definition and Usage:

The `input()` function allows user input.