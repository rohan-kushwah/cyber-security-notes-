  

# 📘 PHP Study Guide: Variables, Datatypes, Reserved Keywords & String Functions

  

---

  

## ❓ What is a variable? Give me a program that holds all data types and define it.

  

A **variable** in PHP is a container used to store data values. Variables start with a `$` sign followed by the name.

  

### ✅ PHP Example with All Data Types:

```php

<?php

// Integer

$age = 25; // Whole number

  

// Float

$price = 99.99; // Decimal number

  

// String

$name = "Amit Kumar"; // Sequence of characters

  

// Boolean

$isLoggedIn = true; // true or false

  

// NULL

$emptyValue = NULL; // No value

  

// Array

$colors = array("Red", "Green", "Blue"); // List of values

  

// Object

class Car {

    public $brand = "Toyota";

    function showBrand() {

        return $this->brand;

    }

}

$myCar = new Car(); // Object instance

  

// Resource

$file = fopen("test.txt", "w"); // External file resource

  

// Output data types

echo "Integer: " . gettype($age) . "<br>";

echo "Float: " . gettype($price) . "<br>";

echo "String: " . gettype($name) . "<br>";

echo "Boolean: " . gettype($isLoggedIn) . "<br>";

echo "NULL: " . gettype($emptyValue) . "<br>";

echo "Array: " . gettype($colors) . "<br>";

echo "Object: " . gettype($myCar) . "<br>";

echo "Resource: " . gettype($file) . "<br>";

  

fclose($file);

?>

```

  

---

  

## ❓ What is a datatype?

  

A **datatype** tells the type of data a variable holds. PHP is a loosely typed language, so variable types are determined at runtime.

  

### 📋 PHP Data Types:

| Datatype   | Example          | Description                          |

|------------|------------------|--------------------------------------|

| Integer    | `$x = 10;`       | Whole number                         |

| Float      | `$x = 3.14;`     | Decimal number                       |

| String     | `$x = "Hello";`  | Text                                 |

| Boolean    | `$x = true;`     | True or False                        |

| NULL       | `$x = NULL;`     | No value                             |

| Array      | `array(1,2,3)`   | List of values                       |

| Object     | `new Class()`    | Object from a class                  |

| Resource   | `fopen()`        | External resource like file/db conn  |

  

---

  

## ❓ List of reserved keywords of PHP

  

Reserved keywords are special words in PHP that **cannot be used** as names for variables, classes, or functions.

  

### 🔒 Reserved Keywords in PHP

  

**Control Structures:**

```

if, else, elseif, endif, while, endwhile, do, for, foreach, endforeach, switch, endswitch, case, default, break, continue, return, goto

```

  

**Functions and Classes:**

```

function, const, class, interface, trait, extends, implements, abstract, final, public, protected, private, static, global, var

```

  

**Others:**

```

namespace, use, new, clone, throw, try, catch, finally, echo, print, isset, empty, die, exit, list, array, match, yield, include, require

```

  

---

  

## ❓ What is PHP String Function with Examples?

  

PHP string functions are used to perform operations on strings like changing case, finding length, replacing text, etc.

  

### ✅ Complete PHP Program for String Functions

```php

<?php

echo "<h2>PHP String Functions Demo</h2>";

  

$text = "Hello PHP";

  

// 1. strlen() - Get length of string

echo "1. Length of '$text': " . strlen($text) . "<br>";

  

// 2. strtoupper() - Convert to uppercase

echo "2. Uppercase: " . strtoupper($text) . "<br>";

  

// 3. strtolower() - Convert to lowercase

echo "3. Lowercase: " . strtolower($text) . "<br>";

  

// 4. strrev() - Reverse the string

echo "4. Reversed: " . strrev($text) . "<br>";

  

// 5. strpos() - Find position of substring

echo "5. Position of 'PHP' in '$text': " . strpos($text, "PHP") . "<br>";

  

// 6. str_replace() - Replace text in string

echo "6. Replace 'PHP' with 'Java': " . str_replace("PHP", "Java", $text) . "<br>";

  

// 7. trim() - Remove spaces

$raw = "   Hello World!   ";

echo "7. Trimmed: '" . trim($raw) . "'<br>";

  

// 8. explode() - Split string into array

$csv = "apple,banana,grape";

$fruits = explode(",", $csv);

echo "8. explode(): ";

print_r($fruits);

echo "<br>";

  

// 9. implode() - Join array into string

$joined = implode(" - ", $fruits);

echo "9. implode(): $joined<br>";

  

// 10. substr() - Get part of a string

echo "10. Substring: " . substr($text, 0, 5) . "<br>";

?>

```

  

### 🔎 Summary Table

  

| Function        | Purpose                   | Example Output           |

|-----------------|---------------------------|---------------------------|

| `strlen()`      | Length of string          | `strlen("Hi") → 2`        |

| `strtoupper()`  | Convert to uppercase      | `"hi" → "HI"`             |

| `strtolower()`  | Convert to lowercase      | `"HI" → "hi"`             |

| `strrev()`      | Reverse string            | `"abc" → "cba"`           |

| `strpos()`      | Find position             | `"Hello PHP" → 6`         |

| `str_replace()` | Replace text              | `"PHP" → "Java"`          |

| `trim()`        | Remove whitespace         | `"  hi  " → "hi"`         |

| `explode()`     | Split string into array   | `"a,b" → ["a", "b"]`      |

| `implode()`     | Join array into string    | `["a","b"] → "a-b"`       |

| `substr()`      | Get part of string        | `"Hello",0,3 → "Hel"`     |

  

---

  # others--
  ### 🧠 **What is PHP?**

**PHP** (Hypertext Preprocessor) is an open-source server-side scripting language primarily used for web development. It allows developers to create dynamic content that interacts with databases. PHP is embedded within HTML and can be used in combination with various web frameworks, databases, and other technologies.

---

### 📌 **Basic PHP Syntax**

PHP syntax is simple and easy to learn. Every PHP file must begin with `<?php` and can optionally end with `?>`. Code statements within PHP files must end with a semicolon (`;`).

php

CopyEdit

`<?php echo "Hello, World!";  // Outputs "Hello, World!" ?>`

- PHP code starts with `<?php` and ends with `?>`.
    
- Statements end with a semicolon `;`.
    
- `echo` is used to output data.
    

---

### 💬 **PHP Comments**

PHP supports both single-line and multi-line comments:

- **Single-line Comments:**
    
    php
    
    CopyEdit
    
    `// This is a single-line comment # This is another single-line comment`
    
- **Multi-line Comments:**
    
    php
    
    CopyEdit
    
    `/* This is a    multi-line comment */`
    

---

### 🔢 **PHP Variables and Datatypes**

- **Variables** in PHP are prefixed with a dollar sign `$` followed by a name.
    
- PHP is **loosely typed**, meaning that variable types are determined at runtime based on the value assigned.
    

### Example:

php

CopyEdit

`<?php $age = 25;           // Integer $name = "Amit";      // String $price = 99.99;      // Float $isOnline = true;    // Boolean $colors = array("Red", "Green"); // Array ?>`

---

### 🔍 **PHP Datatypes**

PHP supports several data types for different types of data:

|**Datatype**|**Description**|
|---|---|
|Integer|Whole numbers (e.g., `5`, `-23`)|
|Float|Decimal numbers (e.g., `3.14`, `-0.75`)|
|String|Sequence of characters (e.g., `"Hello"`)|
|Boolean|`true` or `false`|
|NULL|Empty value|
|Array|Collection of values (e.g., `array(1, 2, 3)`)|
|Object|Instances of classes (e.g., `new Class()`)|
|Resource|External resources like file/db connections|

---

### 🔑 **PHP Reserved Keywords**

Reserved keywords are predefined words in PHP that have special meaning and cannot be used as variable, function, or class names.

#### List of Reserved Keywords:

- **Control Structures**: `if`, `else`, `elseif`, `endif`, `while`, `do`, `for`, `foreach`, `switch`, `case`, `default`, `break`, `continue`, `return`, `goto`
    
- **Functions and Classes**: `function`, `const`, `class`, `interface`, `trait`, `extends`, `implements`, `abstract`, `final`, `public`, `protected`, `private`, `static`, `global`, `var`
    
- **Other**: `namespace`, `use`, `new`, `clone`, `throw`, `try`, `catch`, `finally`, `echo`, `print`, `isset`, `empty`, `die`, `exit`, `list`, `array`, `match`, `yield`, `include`, `require`
    

---

### 🔣 **PHP Operators**

Operators are used to perform operations on variables and values. They can be categorized into the following types:

#### Arithmetic Operators:

php

CopyEdit

`+   // Addition -   // Subtraction *   // Multiplication /   // Division %   // Modulus`

#### Comparison Operators:

php

CopyEdit

`==  // Equal to !=  // Not equal to === // Identical (equal and same type) !== // Not identical <   // Less than >   // Greater than <=  // Less than or equal to >=  // Greater than or equal to`

#### Logical Operators:

php

CopyEdit

`&&  // AND ||  // OR !   // NOT`

---

### 🔁 **PHP Control Structures**

#### **If-Else**

Conditional statements that execute blocks of code based on conditions.

php

CopyEdit

`if ($x > 10) {     echo "Greater"; } else {     echo "Smaller"; }`

#### **Switch**

Switch-case is used to execute one out of many blocks of code based on a variable's value.

php

CopyEdit

`switch ($day) {     case "Mon":         echo "Start";         break;     default:         echo "Other day"; }`

#### **Loops**

- **While Loop:**
    
    php
    
    CopyEdit
    
    `$i = 1; while ($i <= 5) {     echo $i;     $i++; }`
    
- **For Loop:**
    
    php
    
    CopyEdit
    
    `for ($i = 0; $i < 5; $i++) {     echo $i; }`
    
- **Foreach (For Arrays):**
    
    php
    
    CopyEdit
    
    `$colors = ["Red", "Green", "Blue"]; foreach ($colors as $color) {     echo $color; }`
    

---

### 🧰 **PHP String Functions**

PHP provides many built-in string functions for operations like length, case conversion, replacing text, and more.

#### Examples:

php

CopyEdit

`$str = " Hello PHP "; echo strlen($str);        // Get the length of the string (11) echo strtoupper($str);    // Convert string to uppercase ("HELLO PHP") echo strtolower($str);    // Convert string to lowercase ("hello php") echo trim($str);          // Remove spaces ("Hello PHP") echo str_replace("PHP", "World", $str);  // Replace "PHP" with "World"`

#### **Common String Functions**:

- `strlen($str)` – Returns the length of the string.
    
- `strtoupper($str)` – Converts string to uppercase.
    
- `strtolower($str)` – Converts string to lowercase.
    
- `trim($str)` – Removes whitespace from both ends of the string.
    
- `str_replace($search, $replace, $subject)` – Replaces occurrences of `$search` with `$replace` in the string.
    

---

### 🧮 **PHP Functions**

Functions are reusable blocks of code that can accept parameters and return values.

php

CopyEdit

`function greet($name) {     return "Hello, $name!"; } echo greet("Amit");  // Outputs: "Hello, Amit!"`

---

### 🗃️ **PHP Arrays**

Arrays are used to store multiple values in a single variable. PHP supports both indexed and associative arrays.

- **Indexed Array:**
    
    php
    
    CopyEdit
    
    `$colors = ["Red", "Green", "Blue"]; echo $colors[0];  // Outputs: Red`
    
- **Associative Array:**
    
    php
    
    CopyEdit
    
    `$age = ["Amit" => 20, "Rahul" => 25]; echo $age["Amit"];  // Outputs: 20`
    
- **Multidimensional Array:**
    
    php
    
    CopyEdit
    
    `$matrix = [     [1, 2],     [3, 4] ]; echo $matrix[1][0];  // Outputs: 3`
    

---

### ✅ **Final Practice File (All-in-One Example)**

php

CopyEdit

`<?php $name = "Amit"; $age = 25; $price = 99.99; $isAdmin = true;  echo "<h1>User Info</h1>"; echo "Name: $name<br>"; echo "Age: $age<br>"; echo "Price: $price<br>"; echo "Is Admin: " . ($isAdmin ? "Yes" : "No") . "<br>";  // Array Example $colors = ["Red", "Green", "Blue"]; foreach ($colors as $c) {     echo "Color: $c<br>"; }  // Function Example function add($a, $b) {     return $a + $b; } echo "Sum: " . add(5, 10); ?>`



# Basics of Programming: Data Types, Variables, and Strings

## What is a Data Type?
A **data type** defines the type of data a variable can hold. It tells the computer how to interpret and store the data.

### Common Data Types:
- **Integer (`int`)**: Whole numbers (e.g., 1, -5, 42)
- **Float (`float`)**: Decimal numbers (e.g., 3.14, -0.01)
- **String (`str`)**: A sequence of characters (e.g., "Hello", "123")
- **Boolean (`bool`)**: True or False values
- **List / Array**: Collection of values (e.g., [1, 2, 3])
- **Dictionary / Map**: Key-value pairs (e.g., {"name": "Alice"})

---

## What is a Variable?
A **variable** is a named container used to store data in a program. It allows data to be reused and manipulated.

### Example (in Python):
```python
age = 25       # 'age' is a variable storing an integer
name = "Alex"  # 'name' is a variable storing a string
Key Points:
Variables can change value.

Variables must follow naming rules (e.g., can't start with a number, no spaces).

What is a String?
A string is a data type used to store text. It is a sequence of characters enclosed in quotes.

Example:
python
Copy
Edit
message = "Hello, world!"
String Functions (Methods):
Strings have built-in functions to manipulate text.

Function	Description	Example
len()	Returns length of the string	len("hello") → 5
.lower()	Converts to lowercase	"Hello".lower() → "hello"
.upper()	Converts to uppercase	"hello".upper() → "HELLO"
.replace(old, new)	Replaces substring	"hi there".replace("hi", "hello") → "hello there"
.find(sub)	Finds index of substring	"apple".find("p") → 1
.strip()	Removes whitespace from ends	" hi ".strip() → "hi"
.split(delimiter)	Splits string into a list	"a,b,c".split(",") → ["a", "b", "c"]

vbnet
Copy
Edit
