[PHP - Arrays](https://www.tutorialspoint.com/php/php_arrays.htm)

An array is a data structure that stores one or more similar type of values in a single value. For example if you want to store 100 numbers then instead of defining 100 variables its easy to define an array of 100 length.

There are three different kind of arrays and each array value is accessed using an ID c which is called array index.

- **Numeric array** − An array with a numeric index. Values are stored and accessed in linear fashion.
- **Associative array** − An array with strings as index. This stores element values in association with key values rather than in a strict linear index order.
- **Multidimensional array** − An array containing one or more arrays and values are accessed using multiple indices

**NOTE** − Built-in array functions is given in function reference PHP Array Functions

# Numeric Array :

These arrays can store numbers, strings and any object but their index will be represented by numbers. By default array index starts from zero.

Following is the example showing how to create and access numeric arrays.

Here we have used **array()** function to create array. This function is explained in function reference.

```php
<html>
   <body>

      <?php
         /* First method to create array. */
         $numbers = array( 1, 2, 3, 4, 5);

         foreach( $numbers as $value ) {
            echo "Value is $value <br />";
         }

         /* Second method to create array. */
         $numbers[0] = "one";
         $numbers[1] = "two";
         $numbers[2] = "three";
         $numbers[3] = "four";
         $numbers[4] = "five";

         foreach( $numbers as $value ) {
            echo "Value is $value <br />";
         }
      ?>

   </body>
</html>
```

This will produce the following result −

```
Value is 1
Value is 2
Value is 3
Value is 4
Value is 5
Value is one
Value is two
Value is three
Value is four
Value is five

```

# Associative Arrays :

The associative arrays are very similar to numeric arrays in term of functionality but they are different in terms of their index. Associative array will have their index as string so that you can establish a strong association between key and values.

To store the salaries of employees in an array, a numerically indexed array would not be the best choice. Instead, we could use the employees names as the keys in our associative array, and the value would be their respective salary.

**NOTE − Don't keep associative array inside double quote while printing otherwise it would not return any value.**

```php
<html>
   <body>

      <?php
         /* First method to associate create array. */
         $salaries = array("mohammad" => 2000, "qadir" => 1000, "zara" => 500);

         echo "Salary of mohammad is ". $salaries['mohammad'] . "<br />";
         echo "Salary of qadir is ".  $salaries['qadir']. "<br />";
         echo "Salary of zara is ".  $salaries['zara']. "<br />";

         /* Second method to create array. */
         $salaries['mohammad'] = "high";
         $salaries['qadir'] = "medium";
         $salaries['zara'] = "low";

         echo "Salary of mohammad is ". $salaries['mohammad'] . "<br />";
         echo "Salary of qadir is ".  $salaries['qadir']. "<br />";
         echo "Salary of zara is ".  $salaries['zara']. "<br />";
      ?>

   </body>
</html>
```

This will produce the following result −

```
Salary of mohammad is 2000
Salary of qadir is 1000
Salary of zara is 500
Salary of mohammad is high
Salary of qadir is medium
Salary of zara is low

```

# Multidimensional Arrays :

A multi-dimensional array each element in the main array can also be an array. And each element in the sub-array can be an array, and so on. Values in the multi-dimensional array are accessed using multiple index.

```php
<html>
   <body>

      <?php
         $marks = array(
            "mohammad" => array (
               "physics" => 35,
               "maths" => 30,
               "chemistry" => 39
            ),

            "qadir" => array (
               "physics" => 30,
               "maths" => 32,
               "chemistry" => 29
            ),

            "zara" => array (
               "physics" => 31,
               "maths" => 22,
               "chemistry" => 39
            )
         );

         /* Accessing multi-dimensional array values */
         echo "Marks for mohammad in physics : " ;
         echo $marks['mohammad']['physics'] . "<br />";

         echo "Marks for qadir in maths : ";
         echo $marks['qadir']['maths'] . "<br />";

         echo "Marks for zara in chemistry : " ;
         echo $marks['zara']['chemistry'] . "<br />";
      ?>

   </body>
</html>
```

This will produce the following result −

```
Marks for mohammad in physics : 35
Marks for qadir in maths : 32
Marks for zara in chemistry : 39
```

# PHP **Array Functions :**

**PHP Array Functions** allow you to interact with and manipulate arrays in various ways. PHP arrays are essential for storing, managing, and operating on sets of variables.

PHP supports simple and multi-dimensional arrays and may be either user created or created by another function.

Following table lists down all the functions related to PHP Array. Here column version indicates the earliest version of PHP that supports the function.
<table class="table table-bordered">
<tbody><tr>
<th class="ts" width="10%">Sr.No</th>
<th class="ts">Function &amp; Description</th>
<th class="ts">Version</th>
</tr>
<tr>
<td class="ts">1</td>
<td><a href="/php/php_function_array.htm">array()</a>
<p>Create an array</p></td>
<td class="ts">4.2.0</td>
</tr>
<tr>
<td class="ts">2</td>
<td><a href="/php/php_function_array_change_key_case.htm">array_change_key_case()</a>
<p>Returns an array with all keys in lowercase or uppercase</p>
</td>
<td class="ts">4.2.0</td>
</tr>
<tr>
<td class="ts">3</td>
<td><a href="/php/php_function_array_chunk.htm">array_chunk()</a>
<p>Splits an array into chunks of arrays</p>
</td>
<td class="ts">4.2.0</td>
</tr>
<tr>
<td class="ts">3</td>
<td><a href="/php/php_function_array_column.htm">array_column()</a>
<p>Return the values from a single column in the input array</p>
</td>
<td class="ts">5.5.0</td>
</tr>
<tr>
<td class="ts">4</td>
<td><a href="/php/php_function_array_combine.htm">array_combine()</a>
<p>Creates an array by using one array for keys and another for its values</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">5</td>
<td><a href="/php/php_function_array_count_values.htm">array_count_values()</a>
<p>Returns an array with the number of occurrences for each value</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">6</td>
<td><a href="/php/php_function_array_diff.htm">array_diff()</a>
<p>Compares array values, and returns the differences</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">7</td>
<td><a href="/php/php_function_array_diff_assoc.htm">array_diff_assoc()</a>
<p>Compares array keys and values, and returns the differences</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">8</td>
<td><a href="/php/php_function_array_diff_key.htm">array_diff_key()</a>
<p>Compares array keys, and returns the differences</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">9</td>
<td><a href="/php/php_function_array_diff_uassoc.htm">array_diff_uassoc()</a>
<p>Compares array keys and values, with an additional user-made function check, and returns the differences</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">10</td>
<td><a href="/php/php_function_array_diff_ukey.htm">array_diff_ukey()</a>
<p>Compares array keys, with an additional user-made function check, and returns the differences</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">11</td>
<td><a href="/php/php_function_array_fills.htm">array_fill()</a>
<p>Fills an array with values</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">12</td>
<td><a href="/php/php_function_array_fill_keys.htm">array_fill_keys()</a>
<p>Fill an array with values, specifying keys</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">13</td>
<td><a href="/php/php_function_array_filter.htm">array_filter()</a>
<p>Filters elements of an array using a user-made function</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">14</td>
<td><a href="/php/php_function_array_flip.htm">array_flip()</a>
<p>Exchanges all keys with their associated values in an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">15</td>
<td><a href="/php/php_function_array_intersect.htm">array_intersect()</a>
<p>Compares array values, and returns the matches</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">16</td>
<td><a href="/php/php_function_array_intersect_assoc.htm">array_intersect_assoc()</a>
<p>Compares array keys and values, and returns the matches</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">17</td>
<td><a href="/php/php_function_array_intersect_key.htm">array_intersect_key()</a>
<p>Compares array keys, and returns the matches</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">18</td>
<td><a href="/php/php_function_array_intersect_uassoc.htm">array_intersect_uassoc()</a>
<p>Compares array keys and values, with an additional user-made function check, and returns the matches</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">19</td>
<td><a href="/php/php_function_array_intersect_ukey.htm">array_intersect_ukey()</a>
<p>Compares array keys, with an additional user-made function check, and returns the matches</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">20</td>
<td><a href="/php/php_function_array_key_exists.htm">array_key_exists()</a>
<p>Checks if the specified key exists in the array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">21</td>
<td><a href="/php/php_function_array_keys.htm">array_keys()</a>
<p>Returns all the keys of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">22</td>
<td><a href="/php/php_function_array_map.htm">array_map()</a>
<p>Sends each value of an array to a user-made function, which returns new values</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">23</td>
<td><a href="/php/php_function_array_merge.htm">array_merge()</a>
<p>Merges one or more arrays into one array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">24</td>
<td><a href="/php/php_function_array_merge_recursive.htm">array_merge_recursive()</a>
<p>Merges one or more arrays into one array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">25</td>
<td><a href="/php/php_function_array_multisort.htm">array_multisort()</a>
<p>Sorts multiple or multi-dimensional arrays</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">26</td>
<td><a href="/php/php_function_array_pad.htm">array_pad()</a>
<p>Inserts a specified number of items, with a specified value, to an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">27</td>
<td><a href="/php/php_function_array_pop.htm">array_pop()</a>
<p>Deletes the last element of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">28</td>
<td><a href="/php/php_function_array_product.htm">array_product()</a>
<p>Calculates the product of the values in an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">29</td>
<td><a href="/php/php_function_array_push.htm">array_push()</a>
<p>Inserts one or more elements to the end of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">30</td>
<td><a href="/php/php_function_array_rand.htm">array_rand()</a>
<p>Returns one or more random keys from an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">31</td>
<td><a href="/php/php_function_array_reduce.htm">array_reduce()</a>
<p>Returns an array as a string, using a user-defined function</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">32</td>
<td><a href="/php/php_function_array_reverse.htm">array_reverse()</a>
<p>Returns an array in the reverse order</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">33</td>
<td><a href="/php/php_function_array_search.htm">array_search()</a>
<p>Searches an array for a given value and returns the key</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">34</td>
<td><a href="/php/php_function_array_shift.htm">array_shift()</a>
<p>Removes the first element from an array, and returns the value of the removed element</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">35</td>
<td><a href="/php/php_function_array_slice.htm">array_slice()</a>
<p>Returns selected parts of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">36</td>
<td><a href="/php/php_function_array_splice.htm">array_splice()</a>
<p>Removes and replaces specified elements of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">37</td>
<td><a href="/php/php_function_array_sum.htm">array_sum()</a>
<p>Returns the sum of the values in an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">38</td>
<td><a href="/php/php_function_array_udiff.htm">array_udiff()</a>
<p>Compares array values in a user-made function and returns an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">39</td>
<td><a href="/php/php_function_array_udiff_assoc.htm">array_udiff_assoc()</a>
<p>Compares array keys, and compares array values in a user-made function, and returns an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">40</td>
<td><a href="/php/php_function_array_udiff_uassoc.htm">array_udiff_uassoc()</a>
<p>Compares array keys and array values in user-made functions, and returns an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">41</td>
<td><a href="/php/php_function_array_uintersect.htm">array_uintersect()</a>
<p>Compares array values in a user-made function and returns an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">42</td>
<td><a href="/php/php_function_array_uintersect_assoc.htm">array_uintersect_assoc()</a>
<p>Compares array keys, and compares array values in a user-made function, and returns an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">43</td>
<td><a href="/php/php_function_array_uintersect_uassoc.htm">array_uintersect_uassoc()</a>
<p>Compares array keys and array values in user-made functions, and returns an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">44</td>
<td><a href="/php/php_function_array_unique.htm">array_unique()</a>
<p>Removes duplicate values from an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">45</td>
<td><a href="/php/php_function_array_unshift.htm">array_unshift()</a>
<p>Adds one or more elements to the beginning of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">46</td>
<td><a href="/php/php_function_array_values.htm">array_values()</a>
<p>Returns all the values of an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">47</td>
<td><a href="/php/php_function_array_walk.htm">array_walk()</a>
<p>Applies a user function to every member of an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">48</td>
<td><a href="/php/php_function_array_walk_recursive.htm">array_walk_recursive()</a>
<p>Applies a user function recursively to every member of an array</p>
</td>
<td class="ts">5</td>
</tr>
<tr>
<td class="ts">49</td>
<td><a href="/php/php_function_arsort.htm">arsort()</a>
<p>Sorts an array in reverse order and maintain index association</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">50</td>
<td><a href="/php/php_function_asort.htm">asort()</a>
<p>Sorts an array and maintain index association</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">51</td>
<td><a href="/php/php_function_compact.htm">compact()</a>
<p>Create array containing variables and their values</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">52</td>
<td><a href="/php/php_function_count.htm">count()</a>
<p>Counts elements in an array, or properties in an object</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">53</td>
<td><a href="/php/php_function_current.htm">current()</a>
<p>Returns the current element in an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">54</td>
<td><a href="/php/php_function_each.htm">each()</a>
<p>Returns the current key and value pair from an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">55</td>
<td><a href="/php/php_function_end.htm">end()</a>
<p>Sets the internal pointer of an array to its last element</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">56</td>
<td><a href="/php/php_function_extract.htm">extract()</a>
<p>Imports variables into the current symbol table from an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">57</td>
<td><a href="/php/php_function_in_array.htm">in_array()</a>
<p>Checks if a specified value exists in an array</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">58</td>
<td><a href="/php/php_function_key.htm">key()</a>
<p>Fetches a key from an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">59</td>
<td><a href="/php/php_function_krsort.htm">krsort()</a>
<p>Sorts an array by key in reverse order</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">60</td>
<td><a href="/php/php_function_ksort.htm">ksort()</a>
<p>Sorts an array by key</p></td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">61</td>
<td><a href="/php/php_function_list.htm">list()</a>
<p>Assigns variables as if they were an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">62</td>
<td><a href="/php/php_function_natcasesort.htm">natcasesort()</a>
<p>Sorts an array using a case insensitive "natural order" algorithm</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">63</td>
<td><a href="/php/php_function_natsort.htm">natsort()</a>
<p>Sorts an array using a "natural order" algorithm</p>
</td>
<td class="ts">4</td>
</tr>
<tr>
<td class="ts">64</td>
<td><a href="/php/php_function_next.htm">next()</a>
<p>Advance the internal array pointer of an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">65</td>
<td><a href="/php/php_function_pos.htm">pos()</a>
<p>Alias of current()</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">66</td>
<td><a href="/php/php_function_prev.htm">prev()</a>
<p>Rewinds the internal array pointer</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">67</td>
<td><a href="/php/php_function_range.htm">range()</a>
<p>Creates an array containing a range of elements</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">68</td>
<td><a href="/php/php_function_reset.htm">reset()</a>
<p>Sets the internal pointer of an array to its first element</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">69</td>
<td><a href="/php/php_function_rsort.htm">rsort()</a>
<p>Sorts an array in reverse order</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">70</td>
<td><a href="/php/php_function_shuffle.htm">shuffle()</a>
<p>Shuffles an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">71</td>
<td><a href="/php/php_function_sizeof.htm">sizeof()</a>
<p>Alias of count()</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">72</td>
<td><a href="/php/php_function_sort.htm">sort()</a>
<p>Sorts an array</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">73</td>
<td><a href="/php/php_function_uasort.htm">uasort()</a>
<p>Sorts an array with a user-defined function and maintain index association</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">74</td>
<td><a href="/php/php_function_uksort.htm">uksort()</a>
<p>Sorts an array by keys using a user-defined function</p>
</td>
<td class="ts">3</td>
</tr>
<tr>
<td class="ts">75</td>
<td><a href="/php/php_function_usort.htm">usort()</a>
<p>Sorts an array by values using a user-defined function</p>
</td>
<td class="ts">3</td>
</tr>
</tbody></table>
# PHP Array Constants :

<table class="table table-bordered">
<tbody><tr>
<th class="ts" width="10%">Sr.No</th>
<th class="ts">Constant &amp; Description</th>
</tr>
<tr>
<td class="ts">1</td>
<td><p><b>CASE_LOWER</b></p>
<p>Used with array_change_key_case() to convert array keys to lower case</p>
</td>
</tr>
<tr>
<td class="ts">2</td>
<td>
<p><b>CASE_UPPER</b></p>
<p>Used with array_change_key_case() to convert array keys to upper case</p>
</td>
</tr>
<tr>
<td class="ts">3</td>
<td>
<p><b>SORT_ASC</b></p>
<p>Used with array_multisort() to sort in ascending order</p>
</td>
</tr>
<tr>
<td class="ts">4</td>
<td>
<p><b>SORT_DESC</b></p>
<p>Used with array_multisort() to sort in descending order</p>
</td>
</tr>
<tr>
<td class="ts">5</td>
<td>
<p><b>SORT_REGULAR</b></p>
<p>Used to compare items normally</p>
</td>
</tr>
<tr>
<td class="ts">6</td>
<td>
<p><b>SORT_NUMERIC</b></p>
<p>Used to compare items numerically</p>
</td>
</tr>
<tr>
<td class="ts">7</td>
<td>
<p><b>SORT_STRING</b></p>
<p>Used to compare items as strings</p>
</td>
</tr>
<tr>
<td class="ts">8</td>
<td>
<p><b>SORT_LOCALE_STRING</b></p>
<p>Used to compare items as strings, based on the current locale</p>
</td>
</tr>
<tr>
<td class="ts">9</td>
<td>
<p><b>COUNT_NORMAL</b></p>
</td>
</tr>
<tr>
<td class="ts">10</td>
<td>
<p><b>COUNT_RECURSIVE</b></p>
</td>
</tr>
<tr>
<td class="ts">11</td>
<td>
<p><b>EXTR_OVERWRITE</b></p>
</td>
</tr>
<tr>
<td class="ts">12</td>
<td>
<p><b>EXTR_SKIP</b></p>
</td>
</tr>
<tr>
<td class="ts">13</td>
<td>
<p><b>EXTR_PREFIX_SAME</b></p>
</td>
</tr>
<tr>
<td class="ts">14</td>
<td>
<p><b>EXTR_PREFIX_ALL</b></p>
</td>
</tr>
<tr>
<td class="ts">15</td>
<td>
<p><b>EXTR_PREFIX_INVALID</b></p>
</td>
</tr>
<tr>
<td class="ts">16</td>
<td>
<p><b>EXTR_PREFIX_IF_EXISTS</b></p>
</td>
</tr>
<tr>
<td class="ts">17</td>
<td><p><b>EXTR_IF_EXISTS</b></p>
</td>
</tr>
<tr>
<td class="ts">18</td>
<td><p><b>EXTR_REFS</b></p>
</td>
</tr>
</tbody></table>
