# 🛡️ User Management System

### Web Application Penetration Test — SQL Injection Module

---

## 📁 Project Structure

```
ums/
├── connection.php
├── register.php
├── login.php
├── protected_page.php
├── profile.php
└── logout.php
```

---

## 🗄️ Database Setup

**Log in to MySQL:**

```bash
mysql -u root -p
```

**Create & select the database:**

```sql
create database ums_db;
use ums_db;
```

**Create the users table:**

```sql
CREATE TABLE users (
    id       INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50)  NOT NULL UNIQUE,
    email    VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    enable   TINYINT(1)   NOT NULL DEFAULT 1
);
```

---

## 🔌 `connection.php`

```php
<?php
$host     = "localhost";
$user     = "root";
$password = "root";
$dbname   = "ums_db";

$connection = mysqli_connect($host, $user, $password, $dbname);

if (!$connection) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
```

---

## 📝 `register.php`

```php
<?php
include("connection.php");

// Registration (Add) user
if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_POST['action']) && $_POST['action'] == 'register') {
    $username        = $_POST['username'];
    $email           = $_POST['email'];
    $password        = $_POST['password'];
    $hashed_password = password_hash($password, PASSWORD_DEFAULT);

    $query = "INSERT INTO users (username, email, password) VALUES ('$username', '$email', '$hashed_password')";

    if (mysqli_query($connection, $query)) {
        echo '<font color="#0000ff">Registration successful! You can now <a href="login.php">login</a>.</font>';
    } else {
        echo '<font color="#ff0000">Error: ' . mysqli_error($connection) . '</font>';
    }
}
?>

<!DOCTYPE html>
<html>
<head><title>User Management</title></head>
<body>
    <div style="margin-top:70px; font-size:23px; text-align:center">
        <h1>User Registration</h1>
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
            <input type="hidden" name="action" value="register" />
            Username: <input type="text"     name="username" required /><br />
            Email:    <input type="email"    name="email"    required /><br />
            Password: <input type="password" name="password" required /><br />
            <input type="submit" value="Register" />
        </form>
    </div>
</body>
</html>
```

---

## 🔐 `login.php`

```php
<?php
include("connection.php");
session_start();

// Login user
if ($_SERVER['REQUEST_METHOD'] == 'POST' && !isset($_POST['action'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];

    $login_query = "SELECT * FROM users WHERE username='$username' AND enable=1 LIMIT 1";
    $result      = mysqli_query($connection, $login_query);

    if ($result && mysqli_num_rows($result) == 1) {
        $user = mysqli_fetch_assoc($result);
        if (password_verify($password, $user['password'])) {
            $_SESSION['username'] = $user['username'];
            header("Location: protected_page.php");
            exit();
        } else {
            echo '<font color="#ff0000">Invalid password.</font>';
        }
    } else {
        echo '<font color="#ff0000">Invalid username.</font>';
    }
}
?>

<!DOCTYPE html>
<html>
<head><title>User Management System</title></head>
<body>
    <div style="margin-top:70px; font-size:23px; text-align:center">
        <h1>User Login</h1>
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
            Username: <input type="text"     name="username" required /><br />
            Password: <input type="password" name="password" required /><br />
            <input type="submit" name="login" value="Login" />
        </form>
    </div>
</body>
</html>
```

---

## 🔒 `protected_page.php`

```php
<?php
session_start();

if (!isset($_SESSION['username'])) {
    header("Location: login.php");
    exit();
}
?>

<!DOCTYPE html>
<html>
<head><title>Protected Page</title></head>
<body>
    <div style="margin-top:70px; font-size:23px; text-align:center">
        <h1>Welcome, <?php echo $_SESSION['username']; ?>!</h1>
        <p>This is a protected page.</p>
        <a href="logout.php">Logout</a>
        <a href="profile.php">Profile</a>
    </div>
</body>
</html>
```

---

## 👤 `profile.php`

```php
<?php
include("connection.php");
session_start();

// Check if the user is logged in; if not, redirect to login page
if (!isset($_SESSION['username'])) {
    header("Location: login.php");
    exit();
}

// Get the current logged-in username from the session
$username = $_SESSION['username'];

// Retrieve user's current profile information from the database
$query  = "SELECT * FROM users WHERE username='$username' LIMIT 1";
$result = mysqli_query($connection, $query);
$user   = mysqli_fetch_assoc($result);

// Handle profile update form submission
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $new_username    = $_POST['username'];
    $new_email       = $_POST['email'];
    $new_password    = $_POST['password'];
    $hashed_password = password_hash($new_password, PASSWORD_DEFAULT);

    // Update user's profile in the database with new data
    $update_query = "UPDATE users SET username='$new_username', email='$new_email', password='$hashed_password' WHERE username='$username'";

    if (mysqli_query($connection, $update_query)) {
        // Update the session username after successful profile update
        $_SESSION['username'] = $new_username;
        echo '<font color="#0000ff">Profile updated successfully!</font>';
    } else {
        echo '<font color="#ff0000">Error: ' . mysqli_error($connection) . '</font>';
    }
}
?>

<!DOCTYPE html>
<html>
<head><title>User Profile</title></head>
<body>
    <div style="margin-top:70px; font-size:23px; text-align:center">
        <h1>User Profile</h1>
        <!-- Profile update form with current user data filled -->
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST">
            Username:     <input type="text"     name="username" value="<?php echo $user['username']; ?>" required /><br />
            Email:        <input type="email"    name="email"    value="<?php echo $user['email'];    ?>" required /><br />
            New Password: <input type="password" name="password" required /><br />
            <input type="submit" value="Update Profile" />
        </form>
        <a href="logout.php">Logout</a>
    </div>
</body>
</html>
```

---

## 🚪 `logout.php`

```php
<?php
// Start or resume the session
session_start();

// Destroy all data registered to the session, logging the user out
session_destroy();

// Redirect the user to the login page after logout
header("Location: login.php");
exit();
?>
```

---

## ⚠️ Second-Order SQL Injection

> **Second-order SQL injection** occurs when user input is initially stored safely in a database, but later the stored data is retrieved and used unsafely in an SQL query, causing a vulnerability.

### How It Works

|Type|Behaviour|
|---|---|
|**First-order**|Unsanitized user input is immediately used in an unsafe SQL query|
|**Second-order**|User input is stored safely at first — no vulnerability at storage time|
||Later, the stored data is retrieved and incorporated into an SQL query without proper sanitization|
||**This later use of stored data leads to SQL injection vulnerabilities**|

### Example Attack Flow

```
1. 🟡 User Input Stored
   └─ Malicious payload saved to DB: '; DROP TABLE users; --

2. 🔴 Later Retrieval
   └─ Stored input fetched and concatenated unsafely:
      SELECT * FROM users WHERE username='$stored_input';

3. 💥 Injection Occurs
   └─ Malicious payload executes during the retrieval query
```

### In This Application

The vulnerability lives in **`profile.php`** — the username from the session (originally stored in the DB) is directly concatenated into the SELECT query:

```php
// ❌ VULNERABLE — second-order SQLi
$query = "SELECT * FROM users WHERE username='$username' LIMIT 1";
```

And in the UPDATE query:

```php
// ❌ VULNERABLE — second-order SQLi
$update_query = "UPDATE users SET username='$new_username', ... WHERE username='$username'";
```

---

## ✅ Prevention

```php
// ✔️ Use prepared statements for ALL queries — both insert AND retrieval

$stmt = $connection->prepare("SELECT * FROM users WHERE username = ? LIMIT 1");
$stmt->bind_param("s", $username);
$stmt->execute();
```

- Use **parameterized queries / prepared statements** consistently — both when inserting data **and** when retrieving and using stored data.
- **Never concatenate** stored data directly into SQL commands without sanitization.

---

> _"Second-order SQL injection arises from safe storage but unsafe later usage of stored data in SQL queries, making it a subtle and dangerous variant of SQL injection attacks."_