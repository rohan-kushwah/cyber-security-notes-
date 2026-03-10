# Delete Records with PHP

---

### delete-records.php

  

```php

delete-records.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

if (mysqli_connect_error()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

  

// 2. Validate ID parameter

if (!isset($_GET['id']) || !is_numeric($_GET['id'])) {

    die("Invalid or missing student ID.");

}

  

$student_id = (int)$_GET['id']; // safely cast to integer

?>

  

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>Delete Record</title>

</head>

<body>

  

    <h1>Delete Record</h1>

  

    <?php

    // 3. Perform deletion using prepared statement

    $query = "DELETE FROM student WHERE id = ?";

    $stmt = mysqli_prepare($connection, $query);

    mysqli_stmt_bind_param($stmt, "i", $student_id);

    mysqli_stmt_execute($stmt);

  

    if (mysqli_stmt_affected_rows($stmt) > 0) {

        echo "Record with ID {$student_id} deleted successfully.";

    } else {

        echo "No record found with ID {$student_id} or deletion failed.";

    }

    mysqli_stmt_close($stmt);

    mysqli_close($connection);

    ?>

  

</body>

</html>

```

  

---

# View Records with PHP

---

### view-records.php

  

```php

view-records.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

if (mysqli_connect_error()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

  

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>View Records with PHP</title>

    <style>

        body { font-family: Arial, sans-serif; padding: 20px; }

        table {

            border-collapse: collapse;

            width: 100%;

            margin-top: 20px;

        }

        th, td {

            border: 1px solid #444;

            padding: 8px;

            text-align: left;

        }

        th {

            background-color: #eee;

        }

    </style>

</head>

<body>

  

    <h1>View Records with PHP</h1>

    <table>

        <thead>

            <tr>

                <th>ID</th>

                <th>NAME</th>

                <th>AGE</th>

                <th>GENDER</th>

                <th>EMAIL</th>

                <th>COURSE NAME</th>

                <th>CITY NAME</th>

            </tr>

        </thead>

        <tbody>

            <?php

            // 2. Perform database query

            $query = "

                SELECT u.id, u.name, u.age, u.gender, u.email,

                co.course_name, ci.city_name

                FROM users u

                INNER JOIN course co ON u.course_name = co.course_id

                INNER JOIN city ci ON u.city_name = ci.city_id

            ";

  

            $result = mysqli_query($connection, $query);

  

            if (!$result) {

                die("Database Query Failed: " . mysqli_error($connection));

            }

            // 3. Use returned data

            while ($row = mysqli_fetch_assoc($result)) {

                echo "<tr>";

                echo "<td>{$row['id']}</td>";

                echo "<td>{$row['name']}</td>";

                echo "<td>{$row['age']}</td>";

                echo "<td>{$row['gender']}</td>";

                echo "<td>{$row['email']}</td>";

                echo "<td>{$row['course_name']}</td>";

                echo "<td>{$row['city_name']}</td>";

                echo "</tr>";

            }

            ?>

        </tbody>

    </table>

  

</body>

</html>

  

<?php

// 4. Release returned data

mysqli_free_result($result);

  

// 5. Close database connection

mysqli_close($connection);

?>

```

  

---

# Object Oriented Programming in PHP

  

Object Oriented Programming (OOP) in PHP allows developers to structure code using objects and classes, promoting reusability, scalability, and maintainability.

  

---

  

### 🧱 Class

  

A **class** is a blueprint or template that defines the structure and behavior (data and methods) of an object.

  

```php

class Car {

    public $color;

  

    public function drive() {

        echo "The car is driving.";

    }

}

```

  

---

  

### 🚗 Object

  

An **object** is an instance of a class. You can create many objects from the same class.

  

```php

$myCar = new Car();

$myCar->color = "red";

$myCar->drive();

```

  

---

  

### 🖼️ Member Variables (Properties)

  

Variables defined inside a class. Once the object is created, these become the attributes of the object.

  

```php

class Student {

    public $name;

    public $age;

}

```

  

---

### 🧠 Member Functions (Methods)

  

Functions defined inside a class used to interact with object data.

  

```php

class Student {

    public $name;

  

    public function sayHello() {

        echo "Hello, I'm " . $this->name;

    }

}

```

  

---

### 🧬 Inheritance

  

Allows a class (child) to inherit methods and properties from another class (parent).

  

```php

class Animal {

    public function speak() {

        echo "Animal speaks";

    }

}

  

class Dog extends Animal {

    public function speak() {

        echo "Dog barks";

    }

}

```

  

---

  

### 🙂 Parent Class

  

A **parent class** (base/super class) is the one that is extended by another class.

  

---

  

### 🙂 Child Class

  

A **child class** (subclass/derived class) inherits from the parent class and can override or extend its functionality.

  

---

  

### 🔁 Polymorphism

  

Allows objects of different classes to be treated through the same interface, usually via method overriding or interface implementation.

  

```php

class Shape {

    public function draw() {

        echo "Drawing shape";

    }

}

  

class Circle extends Shape {

    public function draw() {

        echo "Drawing circle";

    }

}

```

  

---

  

### ➕ Overloading

  

PHP supports limited overloading through the `__call()` and `__get()` magic methods (not true overloading like Java/C++).

  

```php

class OverloadTest {

    public function __call($name, $arguments) {

        echo "Method $name called with args: " . implode(', ', $arguments);

    }

}

```

  

---

  

### 🧊 Data Abstraction

  

Hides internal implementation details and only shows essential features.

  

```php

abstract class Shape {

    abstract public function draw();

}

```

  

---

  

### 🧱 Encapsulation

  

Combines data and functions into a single unit (object) and restricts access using access specifiers.

  

```php

class BankAccount {

    private $balance = 0;

  

    public function deposit($amount) {

        $this->balance += $amount;

    }

  

    public function getBalance() {

        return $this->balance;

    }

}

```

  

---

  

### ⚙️ Constructor

  

Special method called automatically when an object is created.

  

```php

class Person {

    public function __construct() {

        echo "Person object created.";

    }

}

```

  

---

  

### 🧹 Destructor

  

Special method called automatically when an object is destroyed or goes out of scope.

  

```php

class Person {

    public function __destruct() {

        echo "Person object destroyed.";

    }

}

```

  

---

