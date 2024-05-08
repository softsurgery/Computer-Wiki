[PHP - Constants Types](https://www.tutorialspoint.com/php/php_constants.htm)

A constant is a name or an identifier for a simple value. A constant value cannot change during the execution of the script. By default, a constant is case-sensitive. By convention, constant identifiers are always uppercase. A constant name starts with a letter or underscore, followed by any number of letters, numbers, or underscores. If you have defined a constant, it can never be changed or undefined.

To define a constant you have to use define() function and to retrieve the value of a constant, you have to simply specifying its name. Unlike with variables, you do not need to have a constant with a $. You can also use the function constant() to read a constant's value if you wish to obtain the constant's name dynamically.

## constant() function :

As indicated by the name, this function will return the value of the constant.

This is useful when you want to retrieve value of a constant, but you do not know its name, i.e. It is stored in a variable or returned by a function.

## constant() example

```php
<?php
   define("MINSIZE", 50);

   echo MINSIZE;
   echo constant("MINSIZE"); // same thing as the previous line
?>
```

**Only scalar data (Boolean, integer, float and string) can be contained in constants.**

## Differences between constants and variables are :

- There is no need to write a dollar sign ($) before a constant, where as in Variable one has to write a dollar sign.
- Constants cannot be defined by simple assignment, they may only be defined using the define() function.
- Constants may be defined and accessed anywhere without regard to variable scoping rules.
- Once the Constants have been set, may not be redefined or undefined.

## Valid and invalid constant names :

```php
// Valid constant names
define("ONE", "first thing");
define("TWO2", "second thing");
define("THREE_3", "third thing");
define("__THREE__", "third value");

// Invalid constant names
define("2TWO", "second thing");

```

## PHP Magic constants :

PHP provides a large number of predefined constants to any script which it runs.

There are five magical constants that change depending on where they are used. For example, the value of **LINE** depends on the line that it's used on in your script. These special constants are case-insensitive and are as follows −

A few "magical" PHP constants are given below −

[Magic constants](https://www.notion.so/11054b46d5cf4bd7a88e005abda28a96?pvs=21)