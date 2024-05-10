[PHP - Math Functions](https://www.tutorialspoint.com/php/php_maths_functions.htm)

To enable mathematical operations, PHP has mathematical (arithmetic) operators and a number of mathematical functions. In this chapter, the following mathematical functions are explained with examples.

## PHP abs() Function

The abs() function is an in-built function in PHP iterpreter. This function accepts any number as argument and returns a positive value, disregarding its sign. Absolute value of any number is always positive.

```php
abs( mixed $num)
```

PHP abs() function returns the absolute value of **num**. If the data type of num is float, its return type will also be float. For integer parameter, the return type is integer.
## PHP ceil() Function

The ceil() function is an in-built function in PHP iterpreter. This function accepts any float number as argument and rounds it up to the next highest integer. This function always returns a float number as the range of float is bigger than that of integer.

```php
ceil ( float $num ) : float
```

PHP ceil() function returns the smallest integer value that is bigger than or equal to given parameter.
## PHP exp() Function

The **exp()** function calculates exponent of e that is Euler Number. PHP has a predefined constant M_E that represents Euler Number and is equal to 2.7182818284590452354. Hence, exp(x) returns 2.7182818284590452354x

This function always returns a float.

```php
exp ( float $arg ) : float
```

PHP exp() function returns the Euler Number e raised to given **arg.** Note that **e** is the base of natural algorithm. The exp() function is the inverse of natural logarithm.
## PHP floor() Function

The floor() function is another in-built function in PHP interpreter. This function accepts any float number as argument and rounds it down to the next lowest integer. This function always returns a float number as the range of float is bigger than that of integer.
```php
floor ( float $num ) : float
```

PHP floor() function returns the largest integer less than or equal to the given parameter.
## PHP intdiv() Function

The intdiv() function returns the integer quotient of two integer parameters. If x/y results in "i" as division and "r" as remainder, then −

```
x = y*i+r
```

In this case, intdiv(x,y) returns "i"
```php
intdiv ( int $x , int $y ) : int
```

The "x" parameter forms numerator part of the division expression, while the "y" parameter forms the denominator part of the division expression.

PHP intdiv() function returns the integer quotient of division of "x" by "y". The return value is positive if both the parameters are positive or both the parameters are negative.
## PHP log10() Function

The **log10** () function calculates the base-10 logarithm of a number. Base-10 logarithm is also called **common** or **standard algorithm**. The log10(x) function calculates log10x. It is related to natural algorithm by the following equation −

```
log10x=logex/loge10 ; So that
log10100=loge100/loge10 = 2
```

In PHP, **log10** is represented by **log10()** function

```php
log10 ( float $arg ) : float
```
PHP log10() function returns the base-10 logarithm of **arg**.

## PHP max() Function

The max () function returns the highest element in an array, or the highest amongst two or more comma separated parameters.

```php
max ( array $values ) : mixed
```

Or,

```php
max ( mixed $value1 [, mixed $... ] ) : mixed
```

- If only one parameter is given, it should be an array of values which may be of same or different types.
- If two or more parameters are given, they should be any comparable values of same or different types.
    
PHP max() function returns the highest value from the array parameter or sequence of values. Standard comparison operators are applicable. If multiple values of different types evaluate as equal (e.g. 0 and 'PHP'), the first parameter to the function will be returned.
## PHP min() Function

The min () function returns the lowest element in an array, or the lowest amongst two or more comma separated parameters.

```php
min ( array $values ) : mixed
```

Or,

```php
min ( mixed $value1 [, mixed $... ] ) : mixed
```

- If only one parameter is given, it should be an array of values which may be of same or different types
- If two or more parameters are given, they should be any comparable values of same or different types

PHP min() function returns the lowest value from the array parameter or sequence of values. Standard comparison operators are applicable. If multiple values of different types evaluate as equal (e.g. 0 and 'PHP'), the first parameter to the function will be returned
## PHP pow() Function

The **pow** () function is used to compute the power of a certain number. It returns xy calculation, also termed as x raised to y. PHP also provides "**" as exponentiation operator.

So, pow(x,y) returns xy which is same as x**y.

```php
pow ( number $base , number $exp ) : number
```

The first parameter is the base to be raised. The second parameter is the power to which base needs to be raised.

PHP pow() function returns the base raised to the power of exp. If both arguments are non-negative integers, the result is returned as integer, otherwise it is returned as a float.
## PHP round() Function

The round() function proves useful in rounding any floating point number upto a desired precision level. Positive precision parameter causes the number to be rounded after the decimal point; whereas with negative precision, rounding occurs before the decimal point. Precision is "0" by default.

For example, round(10.6) returns 11, round(10.2) returns 10. The function always returns a floating point number.

This function also has another optional parameter called **mode** that takes one of the redefined constants described later.

```php
round ( float $value , int $precision , int $mode ) : float
```
### Parameters

- **Value** − A float number to be rounded.
- **Precision** − Number of decimal digits to round to. Default is 0. Positive precision rounds given number after decimal point. Negative precision rounds the given number before decimal point.
- **Mode** − One of the following predefined constants.

|Sr.No|Constant & Description|
|---|---|
|1|**PHP_ROUND_HALF_UP**<br><br>Rounds number away from 0 when it is half way there. Hence, 1.5 becomes 2 and -1.5 to -2|
|2|**PHP_ROUND_HALF_DOWN**<br><br>Rounds number towards 0 when it is half way there. Hence 1.5 becomes 1 and -1.5 to -1|
|3|**PHP_ROUND_HALF_EVEN**<br><br>Rounds the number to nearest even value|
|4|**PHP_ROUND_HALF_ODD**<br><br>Rounds the number to nearest odd value|

PHP round() function returns a float number that by rounding the value to a desired precision.
## PHP sqrt() Function

The sqrt() function returns the square root of a positive float number. Since square root for a negative number is not defined, it returns NAN. This is one of the most commonly used functions. This function always returns a floating point number.

```php
sqrt (float $arg) : float
```

PHP sqrt() function returns the square root of the given arg number. For negative numbers, the function returns NAN.
## Predefined Mathematical Constants

In addition to the above mathematical functions, PHP also has the following list of predefined mathematical constants −

|Constant|Value|Description|
|---|---|---|
|M_PI|3.14159265358979323846|Pi|
|M_E|2.7182818284590452354|Euler Number e|
|M_LOG2E|1.4426950408889634074|log2 e|
|M_LOG10E|0.43429448190325182765|log10 e|
|M_LN2|0.69314718055994530942|loge 2|
|M_LN10|M_LN10 2.30258509299404568402 loge 10|loge 10|
|M_PI_2|1.57079632679489661923|pi/2|
|M_PI_4|0.78539816339744830962|pi/4|
|M_1_PI|0.31830988618379067154|1/pi|
|M_2_PI|0.63661977236758134308|2/pi|
|M_SQRTPI|1.77245385090551602729|sqrt(pi)|
|M_2_SQRTPI|1.12837916709551257390|2/sqrt(pi)|
|M_SQRT2|1.41421356237309504880|sqrt(2)|
|M_SQRT3|1.73205080756887729352|sqrt(3)|
|M_SQRT1_2|0.70710678118654752440|1/sqrt(2)|
|M_LNPI|1.14472988584940017414|loge(pi)|
|M_EULER|0.57721566490153286061|Euler constant|
|PHP_ROUND_HALF_UP|1|Round halves up|
|PHP_ROUND_HALF_DOWN|2|Round halves down|
|PHP_ROUND_HALF_EVEN|3|Round halves to even numbers|
|PHP_ROUND_HALF_ODD|4|Round halves to odd numbers|
|NAN|NAN|Not A Number|
|INF|INF|Infinity|