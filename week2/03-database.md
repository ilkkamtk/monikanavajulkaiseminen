# PHP databases

- mysql and mysqli extensions
- PDO
- Prepared statements

---

## mysql and mysqli extensions

These extensions are used to connect to a MySQL or MariaDB database from PHP.

- mysql extension is deprecated, don't use it
- [mysqli extension](https://www.php.net/manual/en/book.mysqli.php) is the improved version of mysql extension
- mysqli stands for MySQL Improved
- mysqli extension is object-oriented and procedural
- supports prepared statements
- [Quickstart guide](https://www.php.net/manual/en/mysqli.quickstart.dual-interface.php)

---

## PDO

PDO stands for PHP Data Objects and is a database access layer providing a uniform method of access to multiple databases not only MySQL or MariaDB.

- lightweight, consistent interface for accessing databases in PHP
- provides a data-access abstraction layer, which means that, regardless of which database you're using, you use the same functions to issue queries and fetch data
- supports prepared statements

### Connecting to a database

```php
<?php
// dbconfig.php

$host = 'localhost';
$dbname = 'test';
$username = 'root';
$password = '';
?>
```

```php
<?php
// dbConnect.php

require 'dbconfig.php';

$dsn = "mysql:host=$host;dbname=$dbname;charset=utf8";

try {
  $DBH = new PDO($dsn, $user, $pass);
  $DBH->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
} catch(PDOException $e) {
  echo "Could not connect to database.";
  file_put_contents('PDOErrors.txt', 'dbConnect.php - ' . $e->getMessage(), FILE_APPEND);
}
?>
```

- In the above example, we are connecting to a MySQL database named `test` on `localhost` using the username `root` and an empty password and setting the character set to `utf8`.
- The `setAttribute` method is used to set the error mode to `ERRMODE_EXCEPTION`. This means that any error will be converted to an exception which can be caught using a try-catch block.
- If the connection fails, the error message will be written to a file called `PDOErrors.txt`.
- Note that the visibility of try/catch block is different from JavaScript. In JavaScript `$DBH` would not be accessible outside the try block. In PHP `$DBH` is accessible in the whole file.

### Inserting data with prepared statements

Prepared statement in MySQL represents a SQL statement template to be executed. The template is precompiled and can be executed multiple times with different parameters. Prepared statements are also considered as a way to prevent SQL injection attacks.

- Unnamed placeholders:

  ```php
  <?php
  // insertData.php

  require 'dbConnect.php';

  $name = 'John';
  $email = 'john@metropolia.fi';
  $age = 25;

  $sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";

  $data = array($name, $email, $age);

  try {
    $STH = $DBH->prepare($sql);
    $STH->execute($data);
  } catch(PDOException $e) {
    echo "Could not insert data into the database.";
    file_put_contents('PDOErrors.txt', 'insertData.php - ' . $e->getMessage(), FILE_APPEND);
  }

  ?>
  ```

- Named placeholders:

  ```php
  <?php
  // insertData.php

  require 'dbConnect.php';

  $name = 'John';
  $email = 'john@metropolia.fi';
  $age = 25;

  $sql = "INSERT INTO users (name, email, age) VALUES (:name, :email, :age)";

  $data = array('name' => $name, 'email' => $email, 'age' => $age);

  try {
    $STH = $DBH->prepare($sql);
    $STH->execute($data);
  } catch(PDOException $e) {
    echo "Could not insert data into the database.";
    file_put_contents('PDOErrors.txt', 'insertData.php - ' . $e->getMessage(), FILE_APPEND);
  }

  ?>
  ```

- Insert two records with named placeholders:

  ```php

  <?php
  // insertData.php

  require 'dbConnect.php';

  $employees = [
    ['name' => 'John', 'email' => 'john@metropolia.fi', 'age' => 25],
    ['name' => 'Jane', 'email' => 'jane@metropolia.fi', 'age' => 30]
  ];

  $sql = "INSERT INTO users (name, email, age) VALUES (:name, :email, :age)";

  try {
    $STH = $DBH->prepare($sql);
    foreach ($employees as $employee) {
      $STH->execute($employee);
    }
  } catch(PDOException $e) {
    echo "Could not insert data into the database.";
    file_put_contents('PDOErrors.txt', 'insertData.php - ' . $e->getMessage(), FILE_APPEND);
  }

  ?>
  ```

---
