 ## SQL Injection (sqli) Lab Setup

### Create a new database named 'sqli_db'

```sql
CREATE DATABASE sqli_db;
```

### Switch to use the newly created database 'sqli_db'

```sql
USE sqli_db;
```

### Create a table named 'users' with columns for id, name, email, password, and enable status

```sql
CREATE TABLE users (
    id INT,                      -- User ID as an integer
    name VARCHAR(50),            -- User's name, varchar with max length 50
    email VARCHAR(50),           -- User's email, varchar with max length 50
    password VARCHAR(50),        -- User's password, varchar with max length 50
    enable INT(1)                -- Status flag indicating if user is enabled (1) or disabled (0)
);
```

### Insert multiple user records into the 'users' table

```sql
INSERT INTO users(id, name, email, password, enable)
VALUES
    (1,'Krishna','krishna@armour.com','e49r034',1),      -- Enabled user
    (2,'admin','admin@armour.com','password123',1),      -- Enabled admin user
    (3,'ankit','ankit@armour.com','123456',1),           -- Enabled user
    (4,'rahul','rahul@armour.com','123456',1),           -- Enabled user
    (5,'pooja','pooja@armour.com','123456',1),           -- Enabled user
    (6,'Vishal','Vishal@armour.com','12343221',0);       -- Disabled user
```

---

## Database Connection Script (connection.php)

```bash
vim connection.php
```

The PHP code for database connection formatted in Markdown with a description:

```php
<?php

// Database connection parameters
$dbhost = "localhost";
$dbuser = "root";
$dbpass = "root";
$dbname = "sqli_db";

// Create connection to MySQL database using mysqli
$connection = mysqli_connect($dbhost, $dbuser, $dbpass, $dbname);

// Check if connection failed and terminate with an error message
if (mysqli_connect_error()) {
    die("Database Connection Failed: " . mysqli_connect_error());
}

?>
```

**Description:** This script establishes a connection to the MySQL database `sqli_db` on localhost using the username and password "root". It checks for connection errors and stops execution if the connection fails.

---

## Vulnerable PHP Script (error-based-string.php)

```bash
vim error-based-string.php
```

```php
<?php

// Include the database connection script
include('connection.php');

?>

<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Error Based String</title>
</head>
<body>

<div style=" margin-top:70px;color:#FFF; font-size:23px; text-align:center">
  <h1><span class="style4">Error based string</span> <br>
  <font size="3" color="#666666">

    <?php
    
    // Check if 'id' parameter is passed via GET request
    if (isset($_GET['id'])) {
        
        // Assign the 'id' parameter value from the GET request to a variable
        $id = $_GET['id'];
        
        // Construct an SQL query to select user record matching the given id
        $query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";
        
        // Execute the query on the database connection
        $result = mysqli_query($connection, $query);
        
        // If the query fails, terminate with error message displaying MySQL error
        if (!$result) {
            die("Database Query Failed" . print_r(mysqli_error($connection)));
        }
        
        // If successful, fetch the resulting row as an associative array and display user info
        while ($row = mysqli_fetch_assoc($result)) {
            echo '<font color= "#0000ff">';
            echo 'Your ID:' . ' - ' . $row['id'];
            echo "<br>";
            echo 'Your User Name:' . ' - ' . $row['name'];
            echo "</font>";
        }
    }
    
    ?>

  </font>
</div>

</body>
</html>
```

---

## Testing URLs

The URLs you provided appear to be local addresses accessing the "error-based-string.php" page with different user ID parameters:

- `http://192.168.1.31/web-pentest/error-based-string.php?id=1`
- `http://192.168.1.31/web-pentest/error-based-string.php?id=2`
- `http://192.168.1.31/web-pentest/error-based-string.php?id=3`

Based on the earlier code for "error-based-string.php," these URLs will run a query to fetch user details for IDs 1, 2, and 3 respectively from the `users` table in the `sqli_db` database.

---

## Accessing these URLs will:

- **Show user information (ID and username) if the record is found.**
- **If there is a SQL error (e.g., an injection attempt), it will reveal the database error message because of the error reporting in the PHP code.**

This page is likely used to demonstrate or test error-based SQL injection vulnerabilities since it accepts the `id` parameter directly into an SQL query without input sanitization or prepared statements.

---

## id Parameter Behavior

The URLs you provided test the `id` parameter behavior on the error-based injection page with various types of input:

- `id=1` : A normal integer ID that should return the user with ID 1 if existing.
- `id=-1` : A negative number, likely no user with this ID, may lead to no result or error.
- `id=99999999999999999999999999999999999999999999999999999999999999999999` : An extremely large number, probably no matching record, tests how the system handles large values.
- `id=admin` : A string instead of integer, testing type handling and potential error output or injection attempts.

Based on the PHP code, these inputs are directly inserted into the SQL query, so:

- **Numeric values will be used as-is in the query.**
- **String inputs like "admin" will also be used directly, causing the query to become syntactically incorrect, which may result in a displayed SQL error message.**

This is characteristic of an error-based SQL injection vulnerability where database errors help attackers learn about the database structure and behavior.

---

## For instance:

- **The string input will likely produce a SQL syntax error.**
- **Very large numbers might return no result or cause unexpected behavior.**
- **Negative numbers may return no results but should not normally produce an error.**

This interaction helps attackers craft further injection payloads by examining error messages or responses. The URLs test the `id` parameter with various inputs on the error-based SQL injection page:

- `id=1` : Valid user ID, displays user info for ID 1.
- `id=-1` : Negative number, likely returns no result or empty output.
- `id=99999999999999999999999999999999999999999999999999999999999999999999` : Extremely large number, tests handling of out-of-range values, likely no match.
- `id=admin` : String input causing a SQL syntax error, triggering error-based SQL injection output that reveals database error details.

---

## Barrack the query using ' (single quote)

To perform a basic SQL injection "barrack" (also known as "breaking" or testing for SQL injection) using a single quote ('), you can try injecting a single quote into the `id` parameter in the URL like this:

```
http://192.168.1.31/web-pentest/error-based-string.php?id=1'
```

This single quote is intended to break the SQL query string, likely causing a syntax error in the database query, which due to error-based string injection will reveal the database error message. This helps confirm if the application is vulnerable to SQL injection.

If the page returns a SQL error message showing a syntax problem, it confirms the query is concatenating user input unsafely.

---

## Example to test deeper:

```
?id=1' OR '1'='1
```

This condition always evaluates to true and can return all records, if injection is possible.

Always make sure to test such payloads only on authorized and permitted systems.