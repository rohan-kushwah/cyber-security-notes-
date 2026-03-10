# Creating records with PHP

---

### creating-records.php

  

```python

creating-records.php

```

  

```php

<?php

// Create connection

$connection = mysqli_connect("localhost", "root", "root", "rsdb");

  

// Check connection

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

?>

  

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>Creating Records with PHP</title>

</head>

<body>

  

<h1>Creating Records with PHP</h1>

  

<?php

// Perform database insert

$query = "INSERT INTO student(name, age, gender, email, percentage)

          VALUES ('nitin', 21, 'M', 'u1@armour.com', 60)";

  

$result = mysqli_query($connection, $query);

  

if ($result) {

    echo "<p style='color:green;'>Record inserted successfully!</p>";

} else {

    echo "<p style='color:red;'>Database Query Failed: " . mysqli_error($connection) . "</p>";

}

?>

  

</body>

</html>

  

<?php

mysqli_close($connection);

?>

```

  

---

### creating-records-2.php

  

```python

creating-records-2.php

```

  

```html

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Creating Records with PHP 2</title>

</head>

<body>

  

<h1>Creating Records with PHP</h1>

  

<form action="creating-records-process-2.php" method="POST">

    <p>

        <label for="student_name">Name:</label>

        <input type="text" name="student_name" id="student_name" required />

    </p>

    <p>

        <label for="student_age">Age:</label>

        <input type="number" name="student_age" id="student_age" required />

    </p>

    <p>

        Gender:

        <input type="radio" id="male" name="student_gender" value="M" required>

        <label for="male">Male</label>

        <input type="radio" id="female" name="student_gender" value="F">

        <label for="female">Female</label>

    </p>

    <p>

        <label for="student_email">Email:</label>

        <input type="email" name="student_email" id="student_email" required />

    </p>

    <p>

        <label for="student_percentage">Percentage:</label>

        <input type="number" name="student_percentage" id="student_percentage" min="0" max="100" required />

    </p>

    <p>

        <input type="submit" name="submit" value="Save" />

    </p>

</form>

  

</body>

</html>

```

  

### creating-records-process-2.php

  

```python

creating-records-process-2.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Check if connection succeeded

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

  

// 2. Capture form inputs safely

$student_name = $_POST['student_name'] ?? '';

$student_age = $_POST['student_age'] ?? '';

$student_gender = $_POST['student_gender'] ?? '';

$student_email = $_POST['student_email'] ?? '';

$student_percentage = $_POST['student_percentage'] ?? '';

  

// Basic validation (optional but helpful)

if (

    empty($student_name) || empty($student_age) || empty($student_gender) ||

    empty($student_email) || empty($student_percentage)

) {

    die("All fields are required.");

}

  

// 3. Use Prepared Statement to avoid SQL injection

$query = "INSERT INTO student (name, age, gender, email, percentage) VALUES (?, ?, ?, ?, ?)";

$stmt = mysqli_prepare($connection, $query);

  

// Bind parameters (s = string, i = integer)

mysqli_stmt_bind_param($stmt, "sisss", $student_name, $student_age, $student_gender, $student_email, $student_percentage);

  

// Execute and check result

if (mysqli_stmt_execute($stmt)) {

    echo "<h3>Record successfully inserted!</h3>";

    echo "<a href='creating-records-2.php'>Back to Form</a>";

} else {

    echo "Error: " . mysqli_stmt_error($stmt);

}

  

// 4. Close statement and connection

mysqli_stmt_close($stmt);

mysqli_close($connection);

?>

```

  

---


# update records-
# Update-Records with PHP

---

### update-records.php

  

```python

update-records.php

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

    <title>Update records with PHP</title>

</head>

<body>

  

<h1>Update records with PHP</h1>

  

<?php

// 2. Prepare the update query using a prepared statement

$student_id = 16;

$student_name = "user3";

  

$query = "UPDATE student SET name = ? WHERE id = ?";

  

$stmt = mysqli_prepare($connection, $query);

  

if ($stmt) {

    // 3. Bind parameters (s = string, i = integer)

    mysqli_stmt_bind_param($stmt, "si", $student_name, $student_id);

  

    // 4. Execute the statement

    if (mysqli_stmt_execute($stmt)) {

        echo "<p>Record updated successfully!</p>";

    } else {

        echo "<p>Execution failed: " . mysqli_stmt_error($stmt) . "</p>";

    }

  

    // 5. Close the statement

    mysqli_stmt_close($stmt);

} else {

    die("Statement preparation failed: " . mysqli_error($connection));

}

?>

  

</body>

</html>

  

<?php

// 6. Close the database connection

mysqli_close($connection);

?>

```

  

---

### update-records-2.php

  

```python

update-records-2.php

```

  

```html

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>Update Records with PHP</title>

</head>

<body>

  

<h1>Update Records with PHP</h1>

  

<form action="update-records-process-2.php" method="POST">

    <label for="student_id">ID:</label>

    <input type="number" id="student_id" name="student_id" required /> <br />

  

    <label for="student_name">Name:</label>

    <input type="text" id="student_name" name="student_name" required /> <br />

  

    <label for="student_age">Age:</label>

    <input type="number" id="student_age" name="student_age" required /> <br />

  

    <label>Gender:</label>

    <input type="radio" id="male" name="student_gender" value="M" required>

    <label for="male">Male</label>

    <input type="radio" id="female" name="student_gender" value="F">

    <label for="female">Female</label> <br />

  

    <label for="student_email">Email:</label>

    <input type="email" id="student_email" name="student_email" required /> <br />

  

    <label for="student_percentage">Percentage:</label>

    <input type="number" id="student_percentage" name="student_percentage" step="0.01" required /> <br />

  

    <input type="submit" name="submit" value="Save" />

</form>

  

</body>

</html>

```

  

### update-records-process-2.php

  

```php

update-records-process-2.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

// Test if connection occurred.

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

  

// 2. Collect POST data safely

$student_id = $_POST['student_id'];

$student_name = $_POST['student_name'];

$student_age = $_POST['student_age'];

$student_gender = $_POST['student_gender'];

$student_email = $_POST['student_email'];

$student_percentage = $_POST['student_percentage'];

  

// 3. Prepare and bind

$query = "UPDATE student

          SET name = ?, age = ?, gender = ?, email = ?, percentage = ?

          WHERE id = ?";

$stmt = mysqli_prepare($connection, $query);

  

if ($stmt) {

    mysqli_stmt_bind_param($stmt, "sisssi",

        $student_name,

        $student_age,

        $student_gender,

        $student_email,

        $student_percentage,

        $student_id

    );

  

    $exec = mysqli_stmt_execute($stmt);

  

    if ($exec) {

        echo "<h2>Record updated successfully!</h2>";

    } else {

        echo "<h2>Failed to update record.</h2>";

    }

  

    mysqli_stmt_close($stmt);

} else {

    echo "<h2>Failed to prepare the statement.</h2>";

}

  

// 4. Close the connection

mysqli_close($connection);

?>

```

  

----

---

### update-records-3.php

  

```php

update-records-3.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

  

// 2. Get the ID from URL (GET parameter)

$student_id = $_GET['id'] ?? '';

  

if (!is_numeric($student_id) || $student_id <= 0) {

    die("Invalid or missing student ID.");

}

  

// 3. Query the student record

$query = "SELECT id, name, age, gender, email, percentage FROM student WHERE id = '{$student_id}'";

$result = mysqli_query($connection, $query);

  

if (!$result || mysqli_num_rows($result) == 0) {

    die("Student record not found.");

}

  

$row = mysqli_fetch_assoc($result);

?>

  

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>Update records with PHP</title>

</head>

<body>

  

<h1>Update Student Record</h1>

  

<form action="update-records-process-3.php" method="POST">

    ID: <input type="text" name="student_id" value="<?php echo htmlspecialchars($row['id']); ?>" readonly /><br />

    NAME: <input type="text" name="student_name" value="<?php echo htmlspecialchars($row['name']); ?>" /><br />

    AGE: <input type="text" name="student_age" value="<?php echo htmlspecialchars($row['age']); ?>" /><br />

    GENDER:

    <input type="radio" id="male" name="student_gender" value="M" <?php if ($row['gender'] == "M") echo "checked"; ?> />

    <label for="male">Male</label>

    <input type="radio" id="female" name="student_gender" value="F" <?php if ($row['gender'] == "F") echo "checked"; ?> />

    <label for="female">Female</label><br />

    EMAIL: <input type="text" name="student_email" value="<?php echo htmlspecialchars($row['email']); ?>" /><br />

    PERCENTAGE: <input type="text" name="student_percentage" value="<?php echo htmlspecialchars($row['percentage']); ?>" /><br />

  

    <input type="submit" name="submit" value="Save" />

</form>

  

</body>

</html>

  

<?php

// Close connection

mysqli_close($connection);

?>

```

### update-records-process-3.php

  

```php

update-records-process-3.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

if (mysqli_connect_errno()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

  

// 2. Collect POST data and basic validation

$student_id = $_POST['student_id'] ?? '';

$student_name = $_POST['student_name'] ?? '';

$student_age = $_POST['student_age'] ?? '';

$student_gender = $_POST['student_gender'] ?? '';

$student_email = $_POST['student_email'] ?? '';

$student_percentage = $_POST['student_percentage'] ?? '';

  

// Validate numeric fields

if (!is_numeric($student_id) || !is_numeric($student_age) || !is_numeric($student_percentage)) {

    die("Invalid numeric input.");

}

  

// Optional: Add more validation like email format or required fields

  

// 3. Prepare and execute the query securely

$query = "UPDATE student SET name = ?, age = ?, gender = ?, email = ?, percentage = ? WHERE id = ?";

$stmt = mysqli_prepare($connection, $query);

  

if (!$stmt) {

    die("Prepared Statement Failed: " . mysqli_error($connection));

}

  

mysqli_stmt_bind_param($stmt, "sisssi", $student_name, $student_age, $student_gender, $student_email, $student_percentage, $student_id);

  

$result = mysqli_stmt_execute($stmt);

  

if (!$result) {

    die("Database Update Failed: " . mysqli_stmt_error($stmt));

}

?>

  

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>Update Success</title>

</head>

<body>

    <h1>Student record updated successfully.</h1>

</body>

</html>

  

<?php

// 4. Close statement and connection

mysqli_stmt_close($stmt);

mysqli_close($connection);

?>

```

---

### update-records-4.php

  

```php

update-records-4.php

```

  

```php

<?php

// 1. Create a database connection

$dbhost = "localhost";

$dbuser = "root";

$dbpass = "000000";

$dbname = "rsdb";

  

$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

  

if(mysqli_connect_error()) {

    die("Database Connection Failed: " . mysqli_connect_error());

}

  

// 2. Validate ID parameter

if (!isset($_GET['id']) || !is_numeric($_GET['id'])) {

    die("Invalid or missing student ID.");

}

  

$student_id = (int)$_GET['id']; // cast to int for safety

  

// 3. Fetch student record

$query = "SELECT id, name, age, gender, email, percentage FROM student WHERE id = ?";

$stmt = mysqli_prepare($connection, $query);

mysqli_stmt_bind_param($stmt, "i", $student_id);

mysqli_stmt_execute($stmt);

$result = mysqli_stmt_get_result($stmt);

  

if(!$result || mysqli_num_rows($result) == 0) {

    die("Student not found.");

}

  

$row = mysqli_fetch_assoc($result);

?>

  

<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <title>Update Record - ID <?php echo $student_id; ?></title>

</head>

<body>

  

    <h1>Update Record</h1>

  

    <form action="update-records-process-2.php" method="POST">

        <input type="hidden" name="student_id" value="<?php echo $row['id']; ?>" />

  

        NAME:<input type="text" name="student_name" value="<?php echo htmlspecialchars($row['name']); ?>" /><br />

        AGE:<input type="number" name="student_age" value="<?php echo $row['age']; ?>" /><br />

  

        GENDER:

        <input type="radio" id="male" name="student_gender" value="M" <?php if ($row['gender'] == "M") echo "checked"; ?>>

        <label for="male">Male</label>

  

        <input type="radio" id="female" name="student_gender" value="F" <?php if ($row['gender'] == "F") echo "checked"; ?>>

        <label for="female">Female</label><br />

  

        EMAIL:<input type="email" name="student_email" value="<?php echo htmlspecialchars($row['email']); ?>" /><br />

        PERCENTAGE:<input type="number" step="0.01" name="student_percentage" value="<?php echo $row['percentage']; ?>" /><br />

  

        <input type="submit" value="Save" />

    </form>

</body>

</html>

  

<?php

mysqli_stmt_close($stmt);

mysqli_close($connection);

?>

```

  

---