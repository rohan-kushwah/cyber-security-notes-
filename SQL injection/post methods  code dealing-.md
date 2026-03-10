# 🔒 SQL Injection Techniques - Security Reference Guide

> **⚠️ Educational Purpose Only**: This document is for security professionals and developers learning to protect applications against SQL injection attacks.

---

## 📋 Table of Contents

1. [GET Method Examples (1-6)](https://claude.ai/chat/1d4eda0a-b2b5-4849-94a2-8511470c744c#get-method-examples)
2. [POST Method Examples (7-10)](https://claude.ai/chat/1d4eda0a-b2b5-4849-94a2-8511470c744c#post-method-examples)
3. [Advanced Techniques (11-12)](https://claude.ai/chat/1d4eda0a-b2b5-4849-94a2-8511470c744c#advanced-techniques)

---

## 🔍 GET Method Examples

### 1. Single Quotes Around User Input (Common Vulnerable Style)

```php
$id = $_GET['id'];
$query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";
```

**Vulnerability**: User can inject SQL by breaking out of quotes.

---

### 2. No Quotes (Numeric Input - Risky)

```php
$id = $_GET['id'];
$query = "SELECT * FROM users WHERE id=$id LIMIT 0,1";
```

**Vulnerability**: Even without quotes, numeric context can be exploited.

---

### 3. User Input Wrapped in Parentheses with Single Quotes

```php
$id = $_GET['id'];
$query = "SELECT * FROM users WHERE id=('$id') LIMIT 0,1";
```

**Vulnerability**: Parentheses don't provide protection against injection.

---

### 4. User Input Wrapped in Parentheses Without Quotes

```php
$id = $_GET['id'];
$query = "SELECT * FROM users WHERE id=($id) LIMIT 0,1";
```

**Vulnerability**: Similar to #2, numeric context is still vulnerable.

---

### 5. User Input Wrapped in Double Quotes Inside Parentheses

```php
$id = $_GET['id'];
$id2 = '"' . $id . '"';
$query = "SELECT * FROM users WHERE id=($id2) LIMIT 0,1";
```

**Vulnerability**: Double quotes can be escaped or bypassed.

---

### 6. User Input Concatenated Inside Parentheses with Double Quotes

```php
$id = $_GET['id'];
$id2 = '"' . $id . '"';
$query = "SELECT * FROM users WHERE id=$id2 LIMIT 0,1";
```

**Vulnerability**: Concatenation doesn't sanitize input.

---

## 📮 POST Method Examples

### 7. Using Concatenation with Single Quotes and . Operator

```php
<?php
// Example 7: POST method with concatenation using single quotes and . operator

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $id = $_POST['id'];
    
    // Vulnerable query construction
    $query = 'SELECT * FROM users WHERE id=\'' . $id . '\' LIMIT 0,1';
    
    // Execute query (vulnerable)
    $result = mysqli_query($conn, $query);
    
    if ($result) {
        $row = mysqli_fetch_assoc($result);
        echo "User found: " . $row['username'];
    } else {
        echo "Error: " . mysqli_error($conn);
    }
}
?>

<!-- HTML Form -->
<form method="POST" action="">
    <input type="text" name="id" placeholder="Enter User ID">
    <input type="submit" value="Submit">
</form>
```

**Attack Vector**: `' OR '1'='1` - Bypasses authentication **Resulting Query**: `SELECT * FROM users WHERE id='' OR '1'='1' LIMIT 0,1`

---

### 8. Using Addslashes to Escape Quotes (Insecure & Deprecated)

```php
<?php
// Example 8: POST method using addslashes (insecure method)

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $id = addslashes($_POST['id']);
    
    // Still vulnerable despite addslashes
    $query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";
    
    // Execute query
    $result = mysqli_query($conn, $query);
    
    if ($result) {
        $row = mysqli_fetch_assoc($result);
        echo "User Data: ";
        print_r($row);
    } else {
        echo "Query failed: " . mysqli_error($conn);
    }
}
?>

<!-- HTML Form -->
<form method="POST" action="">
    <label>User ID:</label>
    <input type="text" name="id" placeholder="Enter ID">
    <button type="submit">Search User</button>
</form>
```

**⚠️ Warning**: `addslashes()` is deprecated and can be bypassed with multi-byte character exploits. **Better Alternative**: Use prepared statements with PDO or MySQLi.

---

### 9. Input Cast to Integer (Simple Validation)

```php
<?php
// Example 9: POST method with integer casting

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Cast input to integer for validation
    $id = (int)$_POST['id'];
    
    // Query construction
    $query = "SELECT * FROM users WHERE id=$id LIMIT 0,1";
    
    // Execute query
    $result = mysqli_query($conn, $query);
    
    if ($result && mysqli_num_rows($result) > 0) {
        $row = mysqli_fetch_assoc($result);
        echo "<h3>User Information</h3>";
        echo "<p>ID: " . $row['id'] . "</p>";
        echo "<p>Username: " . $row['username'] . "</p>";
        echo "<p>Email: " . $row['email'] . "</p>";
    } else {
        echo "No user found with ID: " . $id;
    }
}
?>

<!-- HTML Form -->
<form method="POST" action="">
    <div>
        <label for="userId">Enter User ID (numeric):</label>
        <input type="number" id="userId" name="id" required>
    </div>
    <button type="submit">Fetch User</button>
</form>
```

**✅ Pros**: Protects against SQL injection for numeric IDs. **⚠️ Cons**: Only works for numeric inputs; doesn't help with string-based queries.

---

### 10. Using sprintf() (Unsafe Without Validation)

```php
<?php
// Example 10: POST method using sprintf (vulnerable without proper sanitization)

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $id = $_POST['id'];
    
    // Using sprintf to format query (still vulnerable!)
    $query = sprintf("SELECT * FROM users WHERE id='%s' LIMIT 0,1", $id);
    
    // Execute query
    $result = mysqli_query($conn, $query);
    
    if ($result) {
        if (mysqli_num_rows($result) > 0) {
            $row = mysqli_fetch_assoc($result);
            echo "<div class='result'>";
            echo "<h3>User Profile</h3>";
            echo "<p><strong>ID:</strong> " . htmlspecialchars($row['id']) . "</p>";
            echo "<p><strong>Username:</strong> " . htmlspecialchars($row['username']) . "</p>";
            echo "<p><strong>Email:</strong> " . htmlspecialchars($row['email']) . "</p>";
            echo "</div>";
        } else {
            echo "<p class='error'>No user found!</p>";
        }
    } else {
        echo "<p class='error'>Database error: " . mysqli_error($conn) . "</p>";
    }
}
?>

<!-- HTML Form with Styling -->
<!DOCTYPE html>
<html>
<head>
    <style>
        .form-container {
            max-width: 400px;
            margin: 50px auto;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        .result {
            background: #e8f5e9;
            padding: 15px;
            margin-top: 20px;
            border-radius: 5px;
        }
        .error {
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h2>User Lookup</h2>
        <form method="POST" action="">
            <label for="userId">Enter User ID:</label>
            <input type="text" id="userId" name="id" placeholder="e.g., 1" required>
            <button type="submit">Search</button>
        </form>
    </div>
</body>
</html>
```

**⚠️ Critical**: `sprintf()` does NOT sanitize input - it only formats strings! **Attack Vector**: `1' OR '1'='1` still works **Proper Fix**: Use prepared statements instead

---

## 🔓 Advanced Techniques

### 11. Preparing Part of Query and Appending Input Directly (Vulnerable)

```php
<?php
$query = "SELECT * FROM users WHERE id=";
$id = $_GET['id'];
$query .= "'$id' LIMIT 0,1";
?>
```

**Vulnerability**: Direct concatenation allows injection.

---

### 12. Using HEREDOC Syntax with Embedded Input (Vulnerable)

```php
<?php
$id = $_GET['id'];
$query = <<<SQL
SELECT * FROM users WHERE id='$id' LIMIT 0,1
SQL;
?>
```

**Vulnerability**: HEREDOC doesn't provide any protection; input is still unsanitized.

---

## 🛡️ Secure Coding Practices

### ✅ RECOMMENDED: Using Prepared Statements (MySQLi)

```php
<?php
// Secure example with prepared statements
$stmt = $conn->prepare("SELECT * FROM users WHERE id = ? LIMIT 0,1");
$stmt->bind_param("i", $id);
$stmt->execute();
$result = $stmt->get_result();
?>
```

### ✅ RECOMMENDED: Using PDO with Prepared Statements

```php
<?php
// Secure example with PDO
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = :id LIMIT 0,1");
$stmt->execute(['id' => $id]);
$user = $stmt->fetch();
?>
```

---

## 📚 Key Takeaways

|❌ Vulnerable Practice|✅ Secure Alternative|
|---|---|
|String concatenation|Prepared statements|
|`addslashes()`|PDO/MySQLi parameterized queries|
|Manual escaping|ORM frameworks|
|`sprintf()` without sanitization|Bound parameters|
|Direct `$_GET`/`$_POST` usage|Input validation + prepared statements|

---

## 🎯 Testing for SQL Injection

Common payloads to test:

- `' OR '1'='1`
- `' OR 1=1--`
- `' UNION SELECT NULL--`
- `admin'--`
- `1' AND '1'='2`

---

## 📖 Additional Resources

- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [PHP Manual: Prepared Statements](https://www.php.net/manual/en/mysqli.quickstart.prepared-statements.php)
- [PortSwigger SQL Injection Tutorial](https://portswigger.net/web-security/sql-injection)

---

**⚡ Remember**: The only truly secure way to prevent SQL injection is using **prepared statements** with **parameterized queries**!

---

_Document created for educational and security awareness purposes | Last updated: January 2026_