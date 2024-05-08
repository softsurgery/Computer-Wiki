In PHP, both `constant()` and `define()` are used for defining constants, but they serve slightly different purposes:

1. **define()**: This is a language construct used to define constants at runtime. Constants defined using `define()` can be defined anywhere in your code and are accessible everywhere in your script. Constants defined using `define()` are case-sensitive by default, but you can make them case-insensitive by setting the third parameter to `true`. Once a constant is defined, its value cannot be changed or undefined during the script execution.
```php
define("PI", 3.14); echo PI; // Outputs: 3.14
```
    
- **constant()**: This is a function used to get the value of a constant. It can retrieve the value of a constant even if the constant is defined inside a class or if its name is stored in a variable. It's particularly useful when you need to dynamically access constants.
```php
class MyClass {
	const MY_CONSTANT = "Hello"; 
}  
echo constant('MyClass::MY_CONSTANT'); // Outputs: Hello`
```

Example with a variable:
```php
$constName = 'PI'; echo constant($constName); // Outputs: 3.14`
```

So, `define()` is used to create constants, and `constant()` is used to retrieve the value of a constant, potentially in a dynamic manner.