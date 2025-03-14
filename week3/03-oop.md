# Object oriented programming in PHP

## Object oriented programming

Object-oriented programming (OOP) is a programming paradigm that uses objects and classes. It is based on the concept of "objects", which can contain data and code: data in the form of fields (often known as attributes or properties), and code, in the form of procedures (often known as methods).

### Classes and objects

A class is a blueprint for creating objects. A class is a template for objects, and an object is an instance of a class. When the individual objects are created, they inherit all the properties and behaviors from the class, but each object will have its own unique set of values for the properties.

```php
<?php
class Car {
  public $color;
  public $model;
  public $brand;
  public $year;

  public function __construct($color, $model, $brand, $year) {
    $this->color = $color;
    $this->model = $model;
    $this->brand = $brand;
    $this->year = $year;
  }

  public function start() {
    return "The car is started.";
  }

  public function stop() {
    return "The car is stopped.";
  }
}

$car1 = new Car("pearl white", "CLS 320 CDI", "Mercedes Benz", 2008);

echo $car1->start();
echo "<br>";
echo $car1->stop();
echo "<br>";
echo $car1->color;

// output:
// The car is started.
// The car is stopped.
// pearl white

?>
```

### Constructors

A constructor is a special type of method that is automatically called when an object is created. It is used to initialize the object's properties. The constructor method is always named `__construct()`.

### Public, private, and protected

- `public` - the property or method can be accessed from outside the class
- `private` - the property or method can only be accessed within the class
- `protected` - the property or method can only be accessed within the class and by classes derived from the class.

```php
<?php
class Car {
  public $color;
  private $model;
  protected $brand;
  public $year;

  public function __construct($color, $model, $brand, $year) {
    $this->color = $color;
    $this->model = $model;
    $this->brand = $brand;
    $this->year = $year;
  }

  public function start() {
    return "The car is started.";
  }

  public function stop() {
    return "The car is stopped.";
  }
}

$car1 = new Car("pearl white", "CLS 320 CDI", "Mercedes Benz", 2008);

echo $car1->color; // OK
echo $car1->model; // Fatal error: Uncaught Error: Cannot access private property Car::$model
echo $car1->brand; // Fatal error: Uncaught Error: Cannot access protected property Car::$brand
?>
```

To access private and protected properties and methods, you can use public methods in the class.

```php

<?php
class Car {
  public $color;
  private $model;
  protected $brand;
  public $year;

  public function __construct($color, $model, $brand, $year) {
    $this->color = $color;
    $this->model = $model;
    $this->brand = $brand;
    $this->year = $year;
  }

  public function start() {
    return "The car is started.";
  }

  public function stop() {
    return "The car is stopped.";
  }

  public function getModel() {
    return $this->model;
  }

  public function getBrand() {
    return $this->brand;
  }
}

$car1 = new Car("pearl white", "CLS 320 CDI", "Mercedes Benz", 2008);

echo $car1->getModel(); // CLS 320 CDI
echo $car1->getBrand(); // Mercedes Benz
?>
```

**A common practice is to make the properties private and use public methods to access and modify them. This is known as encapsulation.**

### Inheritance

Inheritance is a mechanism in which one class acquires the properties and behaviors of another class. The class that is inherited from is called the parent class, and the class that inherits is called the child class.

```php
<?php
class Vehicle {
  private $color;
  private $brand;
  private $year;

  public function __construct($color, $brand, $year) {
    $this->color = $color;
    $this->brand = $brand;
    $this->year = $year;
  }

  public function start() {
    return "The vehicle is started.";
  }

  public function stop() {
    return "The vehicle is stopped.";
  }

  public function getColor() {
    return $this->color;
  }
}

class Car extends Vehicle {
  private $model;

  public function __construct($color, $brand, $year, $model) {
    parent::__construct($color, $brand, $year);
    $this->model = $model;
  }

  public function getModel() {
    return $this->model;
  }
}

$car1 = new Car("pearl white", "Mercedes Benz", 2008, "CLS 320 CDI");

echo $car1->start();
echo "<br>";
echo $car1->stop();
echo "<br>";
echo $car1->getColor();
echo "<br>";
echo $car1->getModel();


// output:
// The vehicle is started.
// The vehicle is stopped.
// pearl white
// CLS 320 CDI
?>
```

---

## Namespace

Namespaces are a way of encapsulating items so that they can be uniquely identified. A namespace is a way to avoid name collisions between your code and other people's code.

```php
<?php
namespace MyNamespace;

class MyClass {
  // Class code...
}

function myFunction() {
  // Function code...
}

const MY_CONSTANT = "My constant";

?>
```

To use a class, function, or constant from a namespace, you can use the `use` keyword.

```php
<?php

use MyNamespace\MyClass;

$obj = new MyClass();

// use a function

use MyNamespace\myFunction;

myFunction();

// use a constant

use MyNamespace\MY_CONSTANT;

echo MY_CONSTANT;

?>
```

---

### Example: Creating a class to handle database operations with PDO

```php
<?php
// userClass.php

namespace MediaProject;

class User {
    private $id;
    private $name;
    // Other properties and methods...

    public function __construct($id, $name) {
        $this->id = $id;
        $this->name = $name;
    }

    // Getters and setters...
}
?>
```

```php
<?php
// userDatabaseOps.php

namespace MediaProject;

require 'userClass.php';

class UserDatabaseOps {
    private $DBH;

    public function __construct($DBH) {
        $this->DBH = $DBH;
    }

    public function getUserById($id) {
        $sql = "SELECT * FROM users WHERE id = :id";
        $STH = $this->DBH->prepare($sql);
        $STH->execute(array(':id' => $id));
        $STH->setFetchMode(PDO::FETCH_ASSOC);
        $row = $STH->fetch();
        return new User($row['id'], $row['name']);
    }

    // Other methods...

}
?>
```

Usage:

```php
<?php
// index.php

require 'dbConnect.php';

require 'userDatabaseOps.php';

$userOps = new UserDatabaseOps($DBH);
$user = $userOps->getUserById(1);

echo $user->getName();
?>
```

Using namespaces can help you organize your code and avoid conflicts with other code. For example the `User` class in your code might conflict with the `User` class in another library.

---

### Summary

- OOP is a programming paradigm that uses objects and classes
- A class is a blueprint for creating objects
- An object is an instance of a class
- A constructor is a special type of method that is automatically called when an object is created
- `public`, `private`, and `protected` are access modifiers
- Inheritance is a mechanism in which one class acquires the properties and behaviors of another class

---

## Assignment

Convert the previous assignment to use classes and objects. Create a class for handling database operations with PDO. Create a class for handling media item and user operations. Use namespaces to organize your code.
