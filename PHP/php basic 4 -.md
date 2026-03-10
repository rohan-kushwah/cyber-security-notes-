# PHP Basics: Scope, User Input, and Superglobals

## 1. What is Scope in PHP?

**Scope** refers to where a variable can be accessed or seen in your PHP code.

### Types of Scope:

### 1. Local Scope
Variables declared inside a function are only accessible inside that function.

```php
function test() {
    $x = 10; // local scope
    echo $x;
}
test();
// echo $x; // Error: $x is undefined here
2. Global Scope
Variables declared outside any function are global but not accessible inside functions unless declared as global.

php
Copy
Edit
$x = 5;

function myFunc() {
    global $x;
    echo $x;
}
myFunc(); // Outputs: 5
3. Static Scope
Static variables retain their value between function calls.

php
Copy
Edit
function counter() {
    static $count = 0;
    $count++;
    echo $count;
}
counter(); // 1
counter(); // 2
4. Function Parameters (Argument Scope)
Parameters passed to functions act like local variables inside those functions.

php
Copy
Edit
function greet($name) {
    echo "Hello, $name";
}
greet("Alice"); // Outputs: Hello, Alice
5. Superglobals
Built-in PHP variables accessible everywhere (inside or outside functions) without global.

6. Scope in Easy Words
Local Scope: Variables exist only inside a function (like a person in a locked room).

Global Scope: Variables exist outside functions and everywhere else, but need global keyword inside functions.

Static Scope: Variables inside functions that remember their value between calls.

Function Parameters: Values given to functions to use inside only.

Superglobals: Special variables always accessible anywhere.

3. Ways to Get Data from Users on the Web
4. HTML Forms
Collect user input using forms.

html
Copy
Edit
<form action="submit.php" method="POST">
  <input type="text" name="username">
  <input type="submit" value="Submit">
</form>
2. URL Parameters (GET)
Send data through URL query strings.

bash
Copy
Edit
example.com/search.php?query=apple
3. POST Requests
Send data invisibly (not in URL).

Accessed in PHP with $_POST.

4. File Uploads
html
Copy
Edit
<form action="upload.php" method="POST" enctype="multipart/form-data">
  <input type="file" name="myfile">
  <input type="submit">
</form>
5. AJAX (JavaScript + PHP)
Send/receive data without reloading the page.

6. Cookies
Store small data pieces in user browser.

7. Sessions
Store data on the server across multiple pages.

8. JavaScript Prompts
Simple popup input box.

9. What is a Superglobal Variable?
Superglobals are PHP built-in variables accessible anywhere without declaration.

Variable	Description
$_GET	Data from URL query parameters
$_POST	Data from form submissions (POST method)
$_REQUEST	Data from both GET and POST (and COOKIE)
$_SESSION	Store data across user sessions
$_COOKIE	Access cookies
$_FILES	Handle file uploads
$_SERVER	Server and environment info
$_ENV	Environment variables
$GLOBALS	Access all global variables in the script

5. Practical Examples for Superglobals
6. $_GET - Get data from URL
php
Copy
Edit
// URL: example.com/page.php?name=Alice
echo "Hello, " . $_GET['name'];  // Output: Hello, Alice
7. $_POST - Get data from form submission
html
Copy
Edit
<form action="submit.php" method="POST">
  <input type="text" name="email">
  <input type="submit" value="Send">
</form>
php
Copy
Edit
// submit.php
echo "Email received: " . $_POST['email'];
8. $_REQUEST - Works with both GET and POST
php
Copy
Edit
$name = $_REQUEST['name'];
echo "Hi, " . $name;
9. $_SERVER - Server and request info
php
Copy
Edit
echo $_SERVER['SERVER_NAME'];    // e.g., localhost or example.com
echo $_SERVER['REQUEST_METHOD']; // GET or POST
echo $_SERVER['PHP_SELF'];       // Current script name
10. $_FILES - Handling file uploads
html
Copy
Edit
<form action="upload.php" method="POST" enctype="multipart/form-data">
  <input type="file" name="myfile">
  <input type="submit" value="Upload">
</form>
php
Copy
Edit
// upload.php
$filename = $_FILES['myfile']['name'];
$tmp = $_FILES['myfile']['tmp_name'];
move_uploaded_file($tmp, "uploads/" . $filename);
echo "Uploaded: " . $filename;
11. $_COOKIE - Access cookies
php
Copy
Edit
// Set cookie
setcookie("user", "John", time() + 3600); // Expires in 1 hour

// Read cookie
echo "Welcome " . $_COOKIE["user"];
7. $_SESSION - Store session data
php
Copy
Edit
session_start();
$_SESSION["username"] = "Alice";

// On another page
session_start();
echo "Hello, " . $_SESSION["username"];
8. $GLOBALS - Access global variables inside functions
php
Copy
Edit
$x = 10;
$y = 20;

function add() {
  $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
}

add();
echo $z; // Output: 30
