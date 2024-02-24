The Python `print` statement is often used to output variables.

To combine both text and a variable, Python uses the `+` character:

## Example:

```python
x = "awesome"
print("Python is " + x)
```

You can also use the `+` character to add a variable to another variable:

## Example:

```python
x = "Python is "
y = "awesome"
z =  x + y
print(z)
```

For numbers, the `+` character works as a mathematical operator:

## Example:

```python
x = 5
y = 10
print(x + y)
```

If you try to combine a string and a number, Python will give you an error:

## Example:

```python
x = 5
y = "John"
print(x + y)
```