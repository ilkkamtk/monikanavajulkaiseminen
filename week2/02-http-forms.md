# HTTP + Form handling in PHP

## HTTP

- Application layer protocol
  - Relies on TCP/IP
- Stateless protocol
  - No memory of previous requests. Once an HTTP transaction is completed, the connection between the client and server is completely broken: neither the client nor the server has any memory of the transaction
    - Transaction = sequence of the client request and corresponding server response
  - Obvious limitation when developing Web applications
  - Methods have been developed to combine individual transactions into a set of related transactions that perform more complicated tasks.
    - Sessions, cookies, etc.

### HTTP request and response

- [Previous course](https://github.com/ilkkamtk/web-ohjelmoinnin-perusteet/blob/main/http-request-response.md)

### HTTP methods

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

### HTTP status codes

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

---

## Form handling

- HTML form provides a basic way to collect user input
  - PHP provides easy access to the data submitted by the form
- Basic flow:
  - User fills in the form and submits it
  - The data is sent to the server with either GET or POST
    - In both cases the basic format is [query string](https://en.wikipedia.org/wiki/Query_string): `name1=value1&name2=value2`
  - Web server extracts the data and sends it to the PHP interpreter
  - PHP processes the data and usually some HTML is generated as a response
  - The response is sent back to the client

### Accessing form data in PHP

- Form data is accessible through the `$_GET` and `$_POST` superglobal arrays:

  ```html
  <form action="form.php" method="post">
    <input type="text" name="username" />
    <input type="password" name="password" />
    <input type="submit" />
  </form>
  ```

  ```php
  <?php
  $username = $_POST['username'];
  $password = $_POST['password'];

  if (isset($username) && isset($password)) {
    echo "Username: $username, Password: $password";
  }
  ?>
  ```

### Using arrays in forms

- Form elements can be grouped into arrays

  - Useful when you need to read same type of data repeatedly, e.g. from check boxes or option lists with multiple attribute.
  - Example:

    ```html
    <form action="form.php" method="post">
      <label for="color">Choose a color:</label>
      <select name="color[]" id="color" multiple>
        <option value="red">Red</option>
        <option value="green">Green</option>
        <option value="blue">Blue</option>
      </select>
      <input type="submit" />
    </form>
    ```

    ```php
    <?php
    $colors = $_POST['color'];
    if (isset($colors)) {
      foreach ($colors as $color) {
        echo "You chose: $color<br>";
      }
    }
    ?>
    ```

### Using associative arrays in forms

- Example:

  ```html
  <form action="form.php" method="post">
    <input type="text" name="person[name]" />
    <input type="text" name="person[age]" />
    <input type="submit" />
  </form>
  ```

  ```php
  <?php
  $person = $_POST['person'];
  if (isset($person)) {
    echo "Name: " . $person['name'] . ", Age: " . $person['age'];
  }
  ?>
  ```

### Form and the script in the same file

- The form can be in the same file as the script that processes the form data
  - The form is displayed if the page is loaded for the first time
  - The script processes the form data if the form is submitted
- Example:

  ```php
    <?php
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
      $username = $_POST['username'];
      $password = $_POST['password'];
      echo "Username: $username, Password: $password";
    }
    ?>
    <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post">
      <input type="text" name="username">
      <input type="password" name="password">
      <input type="submit">
    </form>
  ```

  - In the example, `$_SERVER['PHP_SELF']` is the path to the current script
  - `$_SERVER['REQUEST_METHOD']` is used to check if the form is submitted or not.

- More commonly the form is in a separate file and the action attribute points to the script that processes the form data.

### Assignment 1

- Create a form that has the following fields:
  - Color (radio buttons), options: red, green, blue
  - Size (select), options: small, medium, large
  - Font style (checkboxes), options: bold, italic
  - Submit button
- Create a script that processes the form data and generates some lorem ipsum text with the selected data as inline styles.
- Use the same file for the form and the script.

---

## Maintaining state between requests

In any realistic interactive web application, you need to maintain information between individual page requests. This is referred to as _maintaining state_ or as the _persistence of data_. HTTP protocol is stateless, so we need to use other methods to maintain state.

Server side languages like PHP provide several methods to maintain state:

- Hidden form fields for simple applications
- Cookies
- Session variables

### Hidden form fields

- Hidden form fields are normal form fields, but they are not visible to the user. This is done by setting the type attribute to "hidden".
- The data is sent to the server when the form is submitted
- The data is accessible through the `$_GET` or `$_POST` superglobal arrays
- Example:

  ```html
  <form action="form.php" method="post">
    <input type="text" name="username" value="John" />
    <input type="hidden" name="id" value="23" />
    <input type="submit" />
  </form>
  ```

  ```php
  // form.php
  <?php
    $username = $_POST['username'];
    $id = $_POST['id'];
    // get data from database using $id
    // echo the database result
  ?>
  ```

- Hidden form fields are not secure because the user can change the value of the field before submitting the form.
- Hidden form fields are useful for simple applications where security is not an issue.

### Cookies

- A cookie is a small piece of data that is stored on the user's computer by the web browser.
- Data saved in a cookie is automatically sent to the same server where the cookie was created when the user visits the server.
- Should not be used for sensitive and/or permanent data
- Cookies are handy for stroring
  - User/site preferences such as color, font size, etc.
  - Shopping cart contents
  - Session keys for user authentication
- Cookies store variable's name and value, plus additional information such as expiration time and the name of website where the cookie came form.
- Website can modify only its own cookies.
- Cookies are sent between server and client as part of the HTTP header
  - This means that they must be sent before any HTML code. Otherwise, you'll get error messages like "headers already sent".
- Example:

  ```php
    <?php
    $cookie_name = "darkmode";
    $cookie_value = "on";
    setcookie($cookie_name, $cookie_value, time() + (86400 * 30), "/"); // 86400 = 1 day
    ?>
  ```

- `setcookie()` parameters are:
  - name
  - value
  - expiration time (in seconds since Unix epoch)
  - path (optional) = the path on the server in which the cookie will be available on
  - domain (optional) = the domain that the cookie is available to
  - secure (optional) = when TRUE the cookie is transmitted only over a secure HTTPS connection from the client
  - httponly (optional) = when TRUE the cookie will be made accessible only through the HTTP protocol and not through scripting languages such as JavaScript
- To delete a cookie, set the expiration time to a time in the past:

  ```php
  setcookie("darkmode", "", time() - 3600);
  ```

- To read a cookie, use the `$_COOKIE` superglobal:

  ```php
  echo "Cookie 'darkmode' is set!<br>";
  echo "Value is: " . $_COOKIE['darkmode'];
  ```

- To check if a cookie is set, use the `isset()` function:

  ```php
    if (isset($_COOKIE['darkmode'])) {
        echo "Cookie 'darkmode' is set!<br>";
        echo "Value is: " . $_COOKIE['darkmode'];
    } else {
        echo "Cookie 'darkmode' is not set!";
    }
  ```

### Assignment 2

- Create a form that has the following fields:
  - Username
  - Remember me (checkbox)
  - Submit button
- When the form is submitted, check if the "Remember me" checkbox is checked. If it is, create a cookie that stores the username. If it is not, delete the cookie.
- When the page is loaded, check if the cookie is set. If it is, add the username from the cookie to input field's value. Also set the checkbox to checked.
- Use the same file for the form and the script.

---

## Sessions

- A session is another way to preserve data across subsequent HTTP requests
- Unlike cookies, session data is stored on the server
- Session data is deleted when the user closes the browser
- Session data is stored in a temporary directory on the server
- User gets a unique session ID, which is stored in a cookie on the user's computer and this cookie is sent to the server with every request to identify the session(=user)
  - This ID is used to retrieve the data linked to the user in the server
- When a user accesses your site, PHP will check whether a specific session id has been sent with the request. If so, the prior saved environment with the registered variables is recreated (=session is maintained). If not, a new session is created.
- Session variables are accessible through the `$_SESSION` superglobal
- Datatypes that can be stored in session variables are the same as for any other PHP variables like strings, arrays, objects, etc.
- Example:

  ```php
  <?php
  session_start();
  $_SESSION['username'] = 'John';
  $_SESSION['color'] = 'red';
  ?>
  ```

- `session_start()` must be called before any output is sent to the browser
- Readind a session variable:

  ```php
  <?php
  session_start();
  echo "Username: " . $_SESSION['username'];
  echo "Color: " . $_SESSION['color'];
  ?>
  ```

- To delete a session variable, use the `unset()` function:

  ```php
    unset($_SESSION['color']);
  ```

- To destroy a session, use the `session_destroy()` function:
- `session_destroy()` destroys all of the data associated with the current session. It does not unset any of the global variables associated with the session, or unset the session cookie. To use the session variables again, `session_start()` has to be called.

  ```php
  session_destroy();
  ```

- To check if a session variable is set, use the `isset()` function:

  ```php
    if (isset($_SESSION['username'])) {
        echo "Username is set!";
    } else {
        echo "Username is not set!";
    }
  ```

### Assignment 3

- Create a form that has the following fields:
  - Username
  - Remember me (checkbox)
  - Submit button
- When the form is submitted, check if the "Remember me" checkbox is checked. If it is, create a session variable that stores the username. If it is not, delete the session variable.
- When the page is loaded, check if the session variable is set. If it is, fill in the username field with the value from the session variable.
- Use the same file for the form and the script.

---
