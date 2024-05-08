[PHP Inclusion](https://www.geeksforgeeks.org/php-inclusion/)

# Require()

The `require()` function in PHP is basically used to include the contents/code/data of one PHP file to another PHP file. 
During this process, if there are any kind of errors then this `require()` function will pop up a warning along with a fatal error and it will immediately stop the execution of the script. In order to use this `require()` function, we will first need to create two PHP files. 
Using the `require()`function include one PHP file into another one. After that, you will see two PHP files combined into one HTML file.

This example illustrates the basic implementation of the require() Function in PHP.
```php
<html>

<body>
    <h1>Welcome to geeks for geeks!</h1>
    <p>Myself, Gaurav Gandal</p>
    <p>Thank you</p>
    <?php require 'GFG.php'; ?>
</body>

</html>

```

```php
<?php
//GFG.php
	echo "
	<p>visit Again-" . date("Y") . " geeks for geeks.com</p>
	";
?>

```

# Include

The `include()` function in PHP is basically used to include the contents/code/data of one PHP file to another PHP file. 
During this process if there are any kind of errors then this `include()` function will pop up a warning but unlike the `require()` function, it will not stop the execution of the script rather the script will continue its process. 
In order to use this `include()` function, we will first need to create two PHP files. Using the `include()` function, include one PHP file into another one. After that, you will see two PHP files combined into one HTML file. 

This example illustrates the basic implementation of the include() Function in PHP.
```php
<html>

<body>
    <h1>Welcome to geeks for geeks!</h1>
    <p>Myself, Gaurav Gandal</p>

    <p>Thank you</p>
    <?php include 'GFG.php'; ?>
</body>

</html>

```

```php
<?php
//GFG.php
	echo "
	<p>Visit Again; " . date("Y") . " Geeks for geeks.com</p>
	";
?>

```

| `include()`                                                                                                                                                      | `require()`                                                                                                                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The `include()` function does not stop the execution of the script even if any error occurs.                                                                     | The `require()` function will stop the execution of the script when an error occurs.                                                                                                                                 |
| The `include()` function does not give a fatal error.                                                                                                            | The `require()` function gives a fatal error                                                                                                                                                                         |
| The `include()` function is mostly used when the file is not required and the application should continue to execute its process when the file is not found.     | The `require()` function is mostly used when the file is mandatory for the application.                                                                                                                              |
| The `include()` function will only produce a warning  [(E_WARNING)](https://www.geeksforgeeks.org/php-types-of-errors/) and the script will continue to execute. | The `require()` will produce a fatal error (E_COMPILE_ERROR) along with the warning and the script will stop its execution.                                                                                          |
| The `include()` function generate various functions and elements that are reused across many pages taking a longer time for the process completion.              | The `require()` function is more in recommendation and considered better whenever we need to stop the execution in case of availability of file, it also saves time avoiding unnecessary inclusions and generations. |