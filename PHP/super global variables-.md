# PHP Superglobals, URL Encoding, and GET Request Form – Reference

  

## 🧩 PHP Superglobals

  

### 1. `$_GET`

Collects data sent via URL query string.

  

**Example URL:**

```

http://example.com/get-example.php?name=John

```

  

**get-example.php**

```php

<?php

echo "Hello, " . $_GET['name'];

?>

```

  

---

  

### 2. `$_POST`

Collects data from an HTML form using POST method.

  

**form-post.html**

```html

<form action="post-example.php" method="post">

  <input type="text" name="email">

  <input type="submit" value="Submit">

</form>

```

  

**post-example.php**

```php

<?php

echo "Email: " . $_POST['email'];

?>

```

  

---

  

### 3. `$_REQUEST`

Contains combined data from `$_GET`, `$_POST`, and `$_COOKIE`.

  

**form-request.html**

```html

<form action="request-example.php" method="post">

  <input type="text" name="username">

  <input type="submit" value="Submit">

</form>

```

  

**request-example.php**

```php

<?php

echo "Username: " . $_REQUEST['username'];

?>

```

  

---

  

### 4. `$_SERVER`

Provides server and execution environment info.

  

**server-example.php**

```php

<?php

echo "Server Name: " . $_SERVER['SERVER_NAME'] . "<br>";

echo "Request Method: " . $_SERVER['REQUEST_METHOD'];

?>

```

  

---

  

### 5. `$_FILES`

Handles file uploads.

  

**form-file.html**

```html

<form action="file-upload.php" method="post" enctype="multipart/form-data">

  <input type="file" name="myfile">

  <input type="submit" value="Upload">

</form>

```

  

**file-upload.php**

```php

<?php

echo "File name: " . $_FILES['myfile']['name'];

?>

```

  

---

  

### 6. `$_SESSION`

Stores session variables.

  

**session-example.php**

```php

<?php

session_start();

$_SESSION['user'] = "Alice";

echo "User: " . $_SESSION['user'];

?>

```

  

---

  

### 7. `$_COOKIE`

Stores cookie data.

  

**cookie-set.php**

```php

<?php

setcookie("favColor", "blue", time() + 3600);

echo "Cookie set!";

?>

```

  

**cookie-get.php**

```php

<?php

echo "Favorite Color: " . $_COOKIE['favColor'];

?>

```

  

---

  

### 8. `$_ENV`

Environment variables.

  

**env-example.php**

```php

<?php

echo getenv('PATH');

?>

```

  

---

  

### 9. `$GLOBALS`

Access global variables inside functions.

  

**globals-example.php**

```php

<?php

$x = 5;

$y = 10;

  

function add() {

  $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];

}

  

add();

echo "Sum: " . $z;

?>

```

  

---

  

## 🔐 URL Encoding

  

### `urlencode()`

  

```php

<?php

echo urlencode("Hello World!"); // Output: Hello+World%21

?>

```

  

### `rawurlencode()`

  

```php

<?php

echo rawurlencode("Hello World!"); // Output: Hello%20World%21

?>

```

  

### Comparison

  

| Function         | Space becomes | Best For          |

|------------------|---------------|-------------------|

| `urlencode()`     | `+`           | Query strings     |

| `rawurlencode()`  | `%20`         | URL paths         |

  

---

  

## 🌐 HTML GET Request Form Example

  

**form-get.html**

```html

<form action="process-get.php" method="get">

  <label for="name">Enter your name:</label>

  <input type="text" id="name" name="name">

  <br>

  <label for="age">Enter your age:</label>

  <input type="number" id="age" name="age">

  <br>

  <input type="submit" value="Submit">

</form>

```

  

**process-get.php**

```php

<?php

$name = $_GET['name'];

$age = $_GET['age'];

echo "Hello, $name. You are $age years old.";

?>

```

  

---

  

Let me know if you'd like a version with POST handling or file download examples.