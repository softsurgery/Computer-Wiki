[PHP - Recursive Functions](https://www.tutorialspoint.com/php/php_recursive_functions.htm)

A recursive function is such a function that calls itself until a certain condition is satisfied. In PHP, it is possible to defines a recursive function.

- Recursion is used when a certain problem is defined in terms of itself.
- Sometimes, it can be tedious to solve a problem using iterative approach. Recursive approach provides a very concise solution to seemingly complex problems.
- Recursion in PHP is very similar to the one in C and C++.
- Recursive functions are particularly used in traversing nested data structures, and searching or sorting algorithms.
- Binary tree traversal, heap sort and finding shortest route are some of the cases where recursion is used.

## Calculation of Factorial using Recursion

The most popular example of recursion is calculation of factorial. Mathematically factorial is defined as −

```
n! = n × (n-1)!
```

It can be seen that we use factorial itself to define factorial. Hence, this is a fit case to write a recursive function.

Let us expand the above definition for calculation of factorial value of 5

```
5! = 5 × 4!
      5 × 4 × 3!
      5 × 4 × 3 × 2!
      5 × 4 × 3 ×  2 × 1!
      5 × 4 × 3 ×  2 × 1
   = 120
```

While we can perform this calculation using a loop, its recursive function involves successively calling it by decrementing the number till it reaches 1.

Following is the recursive function to calculate factorial.

```php
<?php
   function factorial ($n) {
      if ($n == 1) {
         echo $n . PHP_EOL;
         return 1;
      } else {
         echo "$n * ";
         return $n*factorial($n-1);
      }
   }
   echo "Factorial of 5 = " . factorial(5);
?>
```

It will produce the following **output** −

```
5 * 4 * 3 * 2 * 1
Factorial of 5 = 120
```