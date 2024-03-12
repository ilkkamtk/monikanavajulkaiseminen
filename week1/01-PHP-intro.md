# PHP - Hypertext Preprocessor

- A server-side scripting language designed for web development. First appeared in 1995.
- PHP code is executed on the server, and the result is returned to the browser as plain HTML.
- PHP can be embedded into HTML code, or it can be used in combination with various web template systems, web content
  management systems, and web frameworks.
- PHP embedded in HTML:

```php
<!DOCTYPE html>
<html>
  <head>
      <title>My first PHP page</title>
  </head>
  <body>
  
    <h1>My first PHP page</h1>
    
    <p>
      <?php
      echo "Hello World!";
      ?>
    </p>
  
  </body>
</html>
```

---

## PHP Syntax

- PHP code is executed on the server, and the result is returned to the browser as plain HTML.
- PHP files have a file extension of `.php`.
- PHP code is enclosed in `<?php` and `?>` tags. The end tag is optional at the end of a file.
    - Everything outside of these tags is considered plain HTML and is sent directly to the output without being
      processed
      by the PHP engine.
- PHP statements end with a semicolon `;`
- PHP comments:
    - Single-line comments: `//`
    - Multi-line comments: `/* ... */`
    - Documentation comments: `/** ... */`
    - Example:
  ```php
    <?php
    // This is a single-line comment
    /* This is a multi-line comment
       that spans multiple lines */
    /**
     * This is a documentation comment
     * that can be used to generate documentation
     */
    ?>
    ```
- Printing text:
    - The `echo` statement can be used to output data to the screen.
    - The `print` statement can also be used to output data to the screen, but echo is much more common (and much
      faster).
    - Example:
  ```php
    <?php
    echo "<h3>Hello world!</h3>";
    print "<h3>Hello world!</h3>";
    ?>
    ```

---

## PHP Variables

- A variable starts with the `$` sign, followed by the name of the variable.
- A variable name must start with a letter or the underscore character.
- A variable name cannot start with a number.
- A variable name can only contain alpha-numeric characters and underscores (A-z, 0-9, and _).
- Variable names are case-sensitive (`$myVar` and `$myvar` are two different variables).
- Example:
  ```php
  <?php
  $txt = "Hello world!";
  $x = 5;
  $y = 10.5;
  ?>
  ```
- PHP has a total of eight data types:
    - Four scalar types:
        - boolean
        - integer
        - float (floating-point number, also known as double)
        - string
    - Two compound types:
        - array
        - object
    - And finally two special types:
        - resource
        - NULL

### Variable scope

- The scope of a variable is the context within which it is defined. For the most part, all PHP variables only have a
  single scope.
- The global scope is the outermost scope and is the context within which all other scopes are defined.
- Variables declared outside a function have a global scope and can only be accessed outside a function.
- Variables declared inside a function have a local scope and can only be accessed inside a function.
- Example:
  ```php
  <?php
  $x = 5; // global scope
  function myTest() {
      $y = 10; // local scope
      echo "<p>Variable x inside function is: $x</p>";
      echo "<p>Variable y inside function is: $y</p>";
  }
  myTest();
  echo "<p>Variable x outside function is: $x</p>";
  echo "<p>Variable y outside function is: $y</p>";
  ?>
  ```
    - The output will be:
      ```
      Variable x inside function is: 
      Variable y inside function is: 10
      Variable x outside function is: 5
      Variable y outside function is:
      ```
    - To make x available inside the function, use the `global` keyword.

### Global keyword

- Example:
  ```php
  <?php
  $x = 5;
  $y = 10;
  function myTest() {
      global $x, $y;
      $y = $x + $y;
  }
  myTest();
  echo $y; // outputs 15
  ?>
  ```
- The `global` keyword is used to access a global variable from within a function. To do this, use the `global`
  keyword before the variables (inside the function).
- Especially in larger applications, global variables can be problematic because they can be accessed and modified from
  anywhere in the code. This can make debugging difficult. (However, WordPress relies heavily on global variables.)

---

## PHP Strings

- A string can be any text inside quotes. You can use single or double quotes.
- Example:
  ```php
  <?php
  $txt1 = "Hello world!";
  $txt2 = 'Hello world!';
  ?>
  ```
- Concatenation:
    - The PHP concatenation operator is a dot `.` (instead of `+` in JavaScript or Python).
    - Example:
      ```php
      <?php
      $txt1 = "Hello ";
      $txt2 = "world!";
      echo $txt1 . $txt2;
      ?>
      ```

---

## PHP Constants

- A constant is an identifier (name) for a simple value. The value cannot be changed during the script.
- A valid constant name starts with a letter or underscore (no `$` sign before the constant name).
- To create a constant, use the `define()` function.
- Example:
  ```php
  <?php
  define("GREETING", "Welcome to Metropolia!");
  echo GREETING;
  ?>
  ```
- In PHP 7 ->, you can also use the `const` keyword to define a constant:
     ```php
    <?php
    const GREETING = "Welcome to Metropolia!";
    echo GREETING;
    ?>
    ```

---

## PHP Operators

- PHP has mostly the same operators as JavaScript, Python, etc. The main difference is the concatenation operator `.`
  and the
  spaceship operator `<=>`.
    - Concatenation is used to combine two strings.
    - The spaceship operator is used to compare two expressions. It returns -1, 0, or 1 when `$a` is less than, equal
      to, or
      greater than `$b`. Example:
      ```php
         <?php
         echo 1 <=> 1; // 0
         echo 1 <=> 2; // -1
         echo 2 <=> 1; // 1
         ?>
      ```

---

## PHP Conditional Statements and Loops

- PHP supports the following conditional statements:
    - `if` statement
    - `else` statement
    - `elseif` statement
    - `switch` statement
- The syntax is similar to JavaScript, Python, etc. Example:
  ```php
  <?php
  $t = date("H");
  if ($t < "10") {
      echo "Have a good morning!";
  } elseif ($t < "20") {
      echo "Have a good day!";
  } else {
      echo "Have a good night!";
  }
  ?>
  ```
- PHP supports the following loops:
    - `while` loop
    - `do...while` loop
    - `for` loop
    - `foreach` loop
- While, do...while, and for loops are similar to JavaScript, Python, etc. Example:
  ```php
  <?php
  $x = 1;
  while ($x <= 5) {
      echo "The number is: $x <br>";
      $x++;
  }
  ?>
  ```
    - The `foreach` loop is used to iterate through each key/value pair in an array. Example:
  ```php
    <?php
    $colors = array("red", "green", "blue", "yellow");
    foreach ($colors as $value) {
        echo "$value <br>";
    }
    ?>
    ```

### Colon syntax

- PHP also supports a colon syntax for control structures like `if`, `else`, `elseif`, `for` and `foreach` loops etc.
  ```php
  <?php
  $x = 1;
  if ($x == 1):
      echo "One";
  elseif ($x == 2):
      echo "Two";
  else:
      echo "Other";
  endif;
  ?>
  ```
- The colon syntax is useful when mixing HTML and PHP code, and it is often used in WordPress theme development.
  Example:

    ```php
    <?php
    $colors = array("red", "green", "blue", "yellow");
    ?>
    <ul>
      <?php foreach ($colors as $value): ?>
          <li><?php echo $value; ?></li>
      <?php endforeach; ?>
    </ul>
    ```

---

## PHP Functions

- PHP functions are very similar to JavaScript. Example:
  ```php
  <?php
  function writeMsg() {
      echo "Hello world!";
  }
  writeMsg(); // call the function
  ?>
  ```
    - Arrow functions were introduced in PHP 7.4. Example:
  ```php
    <?php
    $add = fn($a, $b) => $a + $b;
    echo $add(1, 2); // 3
    ?>
    ```
    - However, arrow functions are not widely used in PHP yet.

---

## PHP Arrays

- Also, arrays are very similar to JavaScript. Example:
  ```php
  <?php
  $cars = array("Volvo", "BMW", "Toyota");
  echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
  ?>
  ```
    - PHP 5.4 introduced the short array syntax `[]`. Example:
  ```php
    <?php
    $cars = ["Volvo", "BMW", "Toyota"];
    echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
    ?>
    ```

### Different types of arrays:

- Indexed arrays
    - Simple arrays with numeric keys like in JavaScript.
- Associative arrays
    - Arrays with named keys like in JavaScript objects or Python dictionaries.
      Example: `$age = ["Peter" => "35", "Ben" => "37", "Joe" => "43"];`
    - Accessing values in associative arrays:
      ```php
      <?php
      $age = ["Peter" => "35", "Ben" => "37", "Joe" => "43"];
      echo "Peter is " . $age['Peter'] . " years old.";
      ?>
      ```
- Multidimensional arrays.
    - Arrays containing one or more arrays. Example:
  ```php
    <?php
    $cars = [
        ["Volvo", 22, 18],
        ["BMW", 15, 13],
        ["Saab", 5, 2],
        ["Land Rover", 17, 15],
    ];
  
    echo $cars[0][0] . ": In stock: " . $cars[0][1] . ", sold: " . $cars[0][2] . ".<br>";
    echo $cars[1][0] . ": In stock: " . $cars[1][1] . ", sold: " . $cars[1][2] . ".<br>";
    echo $cars[2][0] . ": In stock: " . $cars[2][1] . ", sold: " . $cars[2][2] . ".<br>";
    echo $cars[3][0] . ": In stock: " . $cars[3][1] . ", sold: " . $cars[3][2] . ".<br>";
    ?>
  
    // output: 
    // Volvo: In stock: 22, sold: 18.
    // BMW: In stock: 15, sold: 13.
    // Saab: In stock: 5, sold: 2.
    ```
- PHP also has a large number of built-in functions to work with arrays. Example:
    - `count()` - returns the number of elements in an array.
    - `sort()` - sorts an indexed array in ascending order.
    - `rsort()` - sorts an indexed array in descending order.
    - `asort()` - sorts an associative array in ascending order, according to the value.
    - `ksort()` - sorts an associative array in ascending order, according to the key.
    - `array_push()` - adds one or more elements to the end of an array.
    - `array_pop()` - deletes the last element of an array.
    - `array_shift()` - deletes the first element of an array.
    - `array_unshift()` - adds one or more elements to the beginning of an array.
    - `array_merge()` - merges one or more arrays into one array.
    - `array_search()` - searches an array for a specific value.
    - `array_key_exists()` - checks an array for a specified key.
    - `in_array()` - checks if a value exists in an array.
    - `array_unique()` - removes duplicate values from an array.
    - `array_slice()` - returns selected parts of an array.
    - `array_splice()` - removes and replaces elements from an array.
    - etc.
    - Example:
    ```php
     <?php
     $cars = ["Volvo", "BMW", "Toyota"];
     echo count($cars); // 3
     sort($cars);
     print_r($cars); // Array ( [0] => BMW [1] => Toyota [2] => Volvo )
     ?>
     ```

#### print_r() and var_dump()

- `print_r()` and `var_dump()` are useful for debugging arrays.
- `print_r()` displays information about a variable in a way that's readable by humans.
- `var_dump()` displays structured information about one or more expressions that includes its type and value.
- Example:
  ```php
  <?php
  $cars = ["Volvo", "BMW", "Toyota"];
  print_r($cars);
  var_dump($cars);
  // output
  // Array ( [0] => Volvo [1] => BMW [2] => Toyota )
  // array(3) { [0]=> string(5) "Volvo" [1]=> string(3) "BMW" [2]=> string(6) "Toyota" }
  ?>
  ```

### Looping arrays

- Arrays are usually looped through using the `foreach` loop. However `for` and `while` loops can also be used. Example:
  ```php
  <?php
  $colors = ["red", "green", "blue", "yellow"];
  foreach ($colors as $value) {
      echo "$value <br>";
  }
  ?>
  ```
- Example with associative arrays:
  ```php
  <?php
  $age = ["Peter" => "35", "Ben" => "37", "Joe" => "43"];
  foreach ($age as $key => $val) {
      echo "Key=" . $key . ", Value=" . $val;
  }
  ?>
  ```
    - Key is the property name and val is the property value.
    - The output will be:
      ```
      Key=Peter, Value=35
      Key=Ben, Value=37
      Key=Joe, Value=43
      ```

### Working with JSON

- PHP has built-in functions to encode and decode JSON data. Example:
  ```php
  <?php
  $person = [
      "name" => "John",
      "age" => 30,
      "city" => "New York"
  ];
  echo json_encode($age);
  ?>
  ```
    - The output will be:
      ```
      {"name":"John","age":30,"city":"New York"}
      ```
    - As you can see, associative arrays are converted to objects when using `json_encode()`.
- Example with decoding:
  ```php
  <?php
  $json = '{"name":"John","age":30,"city":"New York"}';
  print_r(json_decode($json, true));
  ?>
  ```
    - The output will be an associative array:
      ```
        Array
        (
            [name] => John
            [age] => 30
            [city] => New York
        )
      ```
    - The second parameter of `json_decode()` is a boolean that when set to `true` will return objects as associative
      arrays. If that is omitted then the result will be an object and the output will be:
        ```
        stdClass Object
        (
            [name] => John
            [age] => 30
            [city] => New York
        )
        ```
