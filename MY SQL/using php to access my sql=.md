 Using PHP to Access MySQL

---

  

PHP provides multiple APIs to connect and interact with MySQL databases. Choosing the right one depends on your needs, but `mysqli` and `PDO` are the recommended modern options.

  

> [PHP: Choosing a MySQL API](https://www.php.net/manual/en/mysqlinfo.api.choosing.php)

  

## Available PHP MySQL APIs

---

  

| API | Description | Status |

|---|---|---|

| mysql | Original, deprecated MySQL API | Deprecated |

| mysqli | "MySQL Improved" API | Preferred |

| PDO | PHP Data Objects (supports multiple DBs) | Flexible |

  

## CRUD Operations

---

The basic database operations are known as **CRUD**:

  

*   **Create** – Insert new records ( `INSERT` )

*   **Read** – Retrieve data ( `SELECT` )

*   **Update** – Modify existing records ( `UPDATE` )

*   **Delete** – Remove records ( `DELETE` )

  

## PHP Database Interaction in Five Steps (Using MySQLi)

---

### Step 1: Create a Database Connection

```php

$connection = mysqli_connect("localhost", "root", "password", "my_database");

  

// Check connection

if (mysqli_connect_errno()) {

    die("Database connection failed: " . mysqli_connect_error());

}

```

### Step 2: Perform a Database Query

---

```php

$query = "SELECT * FROM users";

$result = mysqli_query($connection, $query);

```

  

### Step 3: Use Returned Data (if any)

---

> Use `mysqli_fetch_row()` or `mysqli_fetch_array()` as alternatives.

  

```php

while ($row = mysqli_fetch_assoc($result)) {

    echo "User: " . $row["username"] . "<br />";

}

```

  

### Step 4: Release Returned Data

---

```php

mysqli_free_result($result);

```

  

### Step 5: Close Database Connection

---

```php

mysqli_close($connection);

```

  

## Common Error Handling

---

```php

if (!$connection) {

    die("Connection failed: " . mysqli_connect_error());

}

```

  

## Summary Cheat Sheet

---

  

| Task                | Function                 |

| ------------------- | ------------------------ |

| Connect to DB       | `mysqli_connect()`       |

| Run a query         | `mysqli_query()`         |

| Fetch result row    | `mysqli_fetch_assoc()`   |

| Free result memory  | `mysqli_free_result()`   |

| Close DB connection | `mysqli_close()`         |

| Check for errors    | `mysqli_connect_error()` |

  

---

---


 PHP + MySQL: Creating a Database Connection

---

This guide walks through connecting to a MySQL database using PHP and the MySQLi extension.

  

### Step-by-Step: Database Connection Script

### connection-db.php

  

```php

connection-db.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "root";

$dbname = "armour_db";

  

// Attempt to connect

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Check connection

if (mysqli_connect_error()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Connection with DB</title>

</head>

<body>

    <h1>Database connection successful!</h1>

</body>

</html>

<?php

// 5. Close the database connection

mysqli_close($connection);

?>

```

  

### Connection making Detection

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "root";

$dbname = "armour_db";

  

// Connect to the database

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Test if connection occurred

if (mysqli_connect_error()) {

  die("Database Connection Failed: " . mysqli_connect_error());

}

?>

```

  

## HTML Output Section

```html

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Connection with DB</title>

</head>

<body>

    <h1>Connection DB</h1>

</body>

</html>

```

  

## Close the Connection

```php

<?php

// 5. Close the database connection

mysqli_close($connection);

?>

```

  

## Best Practices

---

*   Always check for connection errors using `mysqli_connect_error()`.

*   Use `mysqli_close()` to free up server resources.

*   Avoid hardcoding credentials in production; use environment variables or config files instead.

  

## Test Your Setup

---

To test:

1.  Ensure your MySQL server is running.

2.  Ensure `armour_db` exists.

3.  Visit the script in your browser: `http://localhost/connection_db.php`

4.  You should see:

    ```

    Connection DB

    ```

  

If there is a connection error, it will display the error message.

  

---

---

# Retrieving Data from MySQL with PHP (mysqli)

---

This guide walks through using PHP to connect to a MySQL database, run a SELECT query, retrieve rows, and display the results using `mysqli`.

  

## What You'll Learn

* How to connect to MySQL

* How to run a SELECT query with `mysqli_query()`

* How to retrieve rows using `mysqli_fetch_row()`

* How to release memory using `mysqli_free_result()`

* How to inspect raw data using `var_dump()`

  

## Requirements

---

* MySQL database: `armour_db`

* Table: `student`

* PHP installed (e.g., XAMPP, LAMP stack)

  

## Full Script: retrieving-data.php

  

```php

retrieving-data.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "root";

$dbname = "armour_db";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Check connection

if (mysqli_connect_error()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Retrieving Data from Database (MySQL)</title>

</head>

<body>

    <h1>Retrieving Data from Database (MySQL)</h1>

<?php

// 2. Perform database query

$query = "SELECT * FROM student";

$result = mysqli_query($connection, $query);

  

// Check if query was successful

if (!$result) {

    die("Database Query Failed");

}

  

// 3. Use returned data (if any)

while ($row = mysqli_fetch_row($result)) {

    // Output raw row array

    var_dump($row);

    echo "<hr />";

}

  

// 4. Release returned data

mysqli_free_result($result);

?>

</body>

</html>

<?php

// 5. Close database connection

mysqli_close($connection);

?>

```

  

## Explanation of Functions

  

| Function | Purpose |

| :--- | :--- |

| `mysqli_connect()` | Connect to the database |

| `mysqli_query()` | Run a SQL query (SELECT) |

| `mysqli_fetch_row()` | Get a row as a numeric array |

| `var_dump()` | Debug-style output showing data types and values |

| `mysqli_free_result()` | Frees memory used by the result |

| `mysqli_close()` | Closes the DB connection |

  

## Sample Output

  

```

array(4) {

  [0]=>

  string(1) "1"

  [1]=>

  string(5) "Alice"

  [2]=>

  string(5) "Delhi"

  [3]=>

  string(3) "90%"

}

```

Each array represents one row of data from the `student` table.

  

## Best Practices

---

* Prefer `mysqli_fetch_assoc()` if you want column names as array keys.

* Sanitize input if building dynamic queries.

* Use prepared statements for security with `mysqli_prepare()` or switch to PDO.

  

---

# Working with Retrieved Data in PHP (mysqli)

  

This tutorial demonstrates how to use different `mysqli_fetch_*` functions to work with data from a MySQL database and present it in raw format, structured output, and HTML tables.

  

## Requirements

*   PHP with mysqli support

*   MySQL database: `rsdb`

*   Tables: `student`, `users`, `course`, `city`

  

### 1. Fetch Data as Indexed Array (mysqli_fetch_row)

---

#### Description

  

*   Returns each row as a numerically indexed array.

*   Columns are accessed using index numbers: `$row[0]`, `$row[1]`, etc.

  

#### Sample Script

```php

while ($row = mysqli_fetch_row($result)) {

    var_dump($row);

    echo "<hr />";

}

```

  

### 2. Fetch Data as Associative Array (mysqli_fetch_assoc)

---

#### Description

*   Returns each row as an associative array.

*   Columns are accessed using column names: `$row['name']`, `$row['email']`.

  

#### Sample Script (retrieved_data2.php)

```php

while ($row = mysqli_fetch_assoc($result)) {

    var_dump($row);

    echo "<hr />";

}

```

  

### 3. Fetch Data as Both Indexed and Associative Array (mysqli_fetch_array)

---

#### Description

*   Can fetch as:

    *   `MYSQLI_ASSOC` - Associative only

    *   `MYSQLI_NUM` - Numeric only

    *   `MYSQLI_BOTH` (default) - Both

  

#### Sample Script (retrieved_data3.php)

```php

while ($row = mysqli_fetch_array($result, MYSQLI_BOTH)) {

    var_dump($row);

    echo "<hr />";

}

```

  

### 4. Access Fields by Column Name (mysqli_fetch_assoc)

---

#### Description

*   Clean and readable data access using column names.

#### Sample Script (retrieved_data4.php)

  

```php

while ($row = mysqli_fetch_assoc($result)) {

    echo $row['id'];

    echo $row['name'];

    echo $row['age'];

    echo $row['gender'];

    echo $row['email'];

    echo $row['percentage'];

    echo "<br />";

}

```

  

### 5. Displaying Data in HTML Table (retrieved_data5.php)

---

#### Description

*   Embeds PHP inside HTML to display table rows dynamically.

  

#### Sample Script

```html

<table>

    <thead>

        <tr><th>ID</th><th>NAME</th><th>AGE</th><th>GENDER</th><th>EMAIL</th><th>PERCENTAGE</th></tr>

    </thead>

    <tbody>

        <?php

        while ($row = mysqli_fetch_assoc($result)) {

        ?>

        <tr>

            <td><?= $row['id'] ?></td>

            <td><?= $row['name'] ?></td>

            <td><?= $row['age'] ?></td>

            <td><?= $row['gender'] ?></td>

            <td><?= $row['email'] ?></td>

            <td><?= $row['percentage'] ?></td>

        </tr>

        <?php } ?>

    </tbody>

</table>

```

  

### 6. Joining Multiple Tables (retrieved_data6.php)

---

#### Description

*   Uses SQL `JOIN` to pull related data from multiple tables.

  

#### Query:

```sql

SELECT u.id, u.name, u.age, u.gender, u.email,

       co.course_name, ci.city_name

FROM users u

INNER JOIN course co ON u.course_name = co.course_id

INNER JOIN city ci ON u.city_name = ci.city_id;

```

  

#### Sample Table Output Script

```html

<tr>

    <td><?= $row['id'] ?></td>

    <td><?= $row['name'] ?></td>

    <td><?= $row['age'] ?></td>

    <td><?= $row['gender'] ?></td>

    <td><?= $row['email'] ?></td>

    <td><?= $row['course_name'] ?></td>

    <td><?= $row['city_name'] ?></td>

</tr>

```

  

## Summary: Which mysqli_fetch_* to Use?

---

  

| Function | Type of Array | Best For |

| :--- | :--- | :--- |

| `mysqli_fetch_row()` | Numeric | Lightweight iteration |

| `mysqli_fetch_assoc()` | Associative | Readable access by column names |

| `mysqli_fetch_array()` | Both | Flexible, but larger footprint |

  

---

### retrieved-data2.php

  

```php

retrieved-data2.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Test if connection occurred

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Retrieving Data from Database (MySQL)</title>

</head>

<body>

    <h1>Retrieving Data From Database (MySQL)</h1>

  

<?php

// 2. Perform database query

$query = "SELECT * FROM student";

$result = mysqli_query($connection, $query);

  

// Check for query error

if (!$result) {

    die("Database Query Failed");

}

  

// 3. Use returned data (if any)

while ($row = mysqli_fetch_assoc($result)) {

    echo "<pre>";

    var_dump($row);

    echo "</pre><hr />";

}

  

// 4. Release returned data

mysqli_free_result($result);

?>

</body>

</html>

<?php

// 5. Close database connection

mysqli_close($connection);

?>

```

  

---

  

### retrieved-data3.php

  

```php

retrieved-data3.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Test if connection occurred

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Retrieving Data from Database (MySQL)</title>

</head>

<body>

    <h1>Retrieving Data from Database (MySQL)</h1>

  

<?php

// 2. Perform database query

$query = "SELECT * FROM student";

$result = mysqli_query($connection, $query);

  

// 3. Check for query success

if (!$result) {

    die("Database Query Failed: " . mysqli_error($connection));

}

  

// 4. Use returned data

while ($row = mysqli_fetch_array($result, MYSQLI_BOTH)) {

    echo "<pre>";

    var_dump($row);

    echo "</pre><hr />";

}

  

// 5. Release returned data

mysqli_free_result($result);

?>

</body>

</html>

<?php

// 6. Close database connection

mysqli_close($connection);

?>

```

  

---

### retrieved-data4.php

  

```php

retrieved-data4.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Test if connection occurred

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>Retrieving Data from Database (MySQL)</title>

    <style>

        body { font-family: Arial, sans-serif; }

        .student { margin-bottom: 20px; padding: 10px; border-bottom: 1px solid #ccc; }

    </style>

</head>

<body>

    <h1>Retrieving Data from Database (MySQL)</h1>

<?php

// 2. Perform database query

$query = "SELECT * FROM student";

$result = mysqli_query($connection, $query);

  

// 3. Check query success

if (!$result) {

    die("Database Query Failed");

}

  

// 4. Use returned data

while ($row = mysqli_fetch_assoc($result)) {

    echo "<div class='student'>";

    echo "ID: " . htmlspecialchars($row['id']) . "<br />";

    echo "Name: " . htmlspecialchars($row['name']) . "<br />";

    echo "Age: " . htmlspecialchars($row['age']) . "<br />";

    echo "Gender: " . htmlspecialchars($row['gender']) . "<br />";

    echo "Email: " . htmlspecialchars($row['email']) . "<br />";

    echo "Percentage: " . htmlspecialchars($row['percentage']) . "<br />";

    echo "</div>";

}

  

// 5. Release returned data

mysqli_free_result($result);

?>

</body>

</html>

```

  

---

### retrieved-data5.php

  

```php

retrieved-data5.php

```

  

```php

<?php

  // 1. Create database connection

  $dbhost = "localhost";

  $dbuser = "root";

  $dbpass = "000000";

  $dbname = "rsdb";

  

  $connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

  // Test if connection occurred

  if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

  }

?>

  

<!DOCTYPE html>

<html>

<head>

  <meta charset="utf-8">

  <title>Retrieving Data from Database (MySQL)</title>

  <style>

    body {

      font-family: Arial, sans-serif;

      margin: 20px;

    }

  

    table {

      width: 100%;

      border-collapse: collapse;

      margin-top: 20px;

    }

  

    th, td {

      border: 1px solid #ccc;

      padding: 8px;

      text-align: left;

    }

    th {

      background-color: #f4f4f4;

    }

  </style>

</head>

<body>

  

  <h1>Retrieving Data from Database (MySQL)</h1>

  

  <table>

    <thead>

      <tr>

        <th>ID</th>

        <th>Name</th>

        <th>Age</th>

        <th>Gender</th>

        <th>Email</th>

        <th>Percentage</th>

      </tr>

    </thead>

    <tbody>

      <?php

        // 2. Perform database query

        $query = "SELECT * FROM student";

        $result = mysqli_query($connection, $query);

  

        if (!$result) {

          die("Database Query Failed");

        }

  

        // 3. Use returned data

        while ($row = mysqli_fetch_assoc($result)) {

          echo "<tr>";

          echo "<td>" . htmlspecialchars($row['id']) . "</td>";

          echo "<td>" . htmlspecialchars($row['name']) . "</td>";

          echo "<td>" . htmlspecialchars($row['age']) . "</td>";

          echo "<td>" . htmlspecialchars($row['gender']) . "</td>";

          echo "<td>" . htmlspecialchars($row['email']) . "</td>";

          echo "<td>" . htmlspecialchars($row['percentage']) . "</td>";

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

### retrieved-data6.php

  

```php

retrieved-data6.php

```

  

```php

<?php

  // 1. Create a database connection

  $dbhost = "localhost";

  $dbuser = "root";

  $dbpass = "000000";

  $dbname = "rsdb";

  

  $connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

  // Test if connection occurred

  if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

  }

?>

  

<!DOCTYPE html>

<html>

<head>

  <meta charset="utf-8">

  <title>Retrieving Data from Database (MySQL)</title>

  <style>

    body {

      font-family: Arial, sans-serif;

      margin: 20px;

    }

  

    table {

      width: 100%;

      border-collapse: collapse;

      margin-top: 20px;

    }

  

    th, td {

      border: 1px solid #ccc;

      padding: 8px;

      text-align: left;

    }

  

    th {

      background-color: #f4f4f4;

    }

  </style>

</head>

<body>

  

  <h1>Retrieving Data from Database (MySQL)</h1>

  

  <table>

    <thead>

      <tr>

        <th>ID</th>

        <th>Name</th>

        <th>Age</th>

        <th>Gender</th>

        <th>Email</th>

      </tr>

    </thead>

    <tbody>

      <?php

        // 2. Perform database query

        $query = "SELECT * FROM student";

        $result = mysqli_query($connection, $query);

  

        if (!$result) {

          die("Database Query Failed");

        }

  

        // 3. Use returned data

        while ($row = mysqli_fetch_assoc($result)) {

          echo "<tr>";

          echo "<td>" . htmlspecialchars($row['id']) . "</td>";

          echo "<td>" . htmlspecialchars($row['name']) . "</td>";

          echo "<td>" . htmlspecialchars($row['age']) . "</td>";

          echo "<td>" . htmlspecialchars($row['gender']) . "</td>";

          echo "<td>" . htmlspecialchars($row['email']) . "</td>";

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

---