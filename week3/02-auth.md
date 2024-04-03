# PHP - Authentication and Authorization

#### Authentication

Authentication is the process of verifying the identity of a user. It is the process of determining whether someone or something is, in fact, who or what it is declared to be.

#### Authorization

Authorization is the process of determining whether a user has permission to perform a specific action. It is the process of granting or denying access to a network resource.

---

## Basic authentication

Basic authentication is a simple authentication scheme built into the HTTP protocol. It is a simple username and password authentication mechanism.

Example:

```php
<?php
$AUTH_USER = 'admin';
$AUTH_PASS = 'admin';
header('Cache-Control: no-cache, must-revalidate, max-age=0');
$has_supplied_credentials = !(empty($_SERVER['PHP_AUTH_USER']) && empty($_SERVER['PHP_AUTH_PW']));
$is_not_authenticated = (
    !$has_supplied_credentials ||
    $_SERVER['PHP_AUTH_USER'] != $AUTH_USER ||
    $_SERVER['PHP_AUTH_PW']   != $AUTH_PASS
);
if ($is_not_authenticated) {
    header('HTTP/1.1 401 Authorization Required');
    header('WWW-Authenticate: Basic realm="Access denied"');
    exit;
}

?>
```
In the above example, we are checking if the user has supplied credentials. If the user has not supplied credentials or the credentials are incorrect, we return a 401 status code and a `WWW-Authenticate` header with the value `Basic realm="Access denied"`.

Basic authentication should be used only in personal sites or sites that do not require high security. It is not recommended for production sites.

---

## Session-based authentication

Session-based authentication is a common method of authentication in web applications. It uses sessions to store user information after the user logs in.

Example:

```php
<?php
session_start();

global $DBH;
require_once __DIR__ . '/dbConnect.php';

if (isset($_POST['username']) && isset($_POST['password'])) {
    $sql = "SELECT * FROM Users WHERE username = :username";
    $data = [
        'username' => $_POST['username'],
    ];
    $STH = $DBH->prepare($sql);
    $STH->execute($data);
    $user = $STH->fetch(PDO::FETCH_ASSOC);
    if ($user && password_verify($_POST['password'], $user['password'])) {
        $_SESSION['user'] = $user;
        // redirect to secret page
        header('Location: secret.php');
        exit;
    } else {
        echo 'Invalid username or password';
    }
}

?>
```

```php
<?php
// secret.php

session_start();

if (!isset($_SESSION['user'])) {
    header('Location: login.php');
    exit;
}

$user = $_SESSION['user'];
echo 'Welcome ' . $user['username'];
echo 'Your role is ' . $user['role'];

?>
```

In the above example, we are checking if the user has supplied a username and password. If the user has supplied a username and password, we hash the password and check if the username and password match the database. If the username and password match, we store the user information in the session and redirect the user to the secret page. If the username and password do not match, we display an error message.

In the secret page, we check if the user information is stored in the session. If the user information is not stored in the session, we redirect the user to the login page.

---

## Assignment

Add session based authentication and authorization to your application. As a user you are able to add/modify/delete your own media items. As an admin you are able to add/modify/delete all media items.


