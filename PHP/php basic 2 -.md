# PHP Basics: Typecasting, Constants, Control Structures, Operators

  

---

  

## 1. PHP Typecasting

  

**Typecasting** is explicitly converting a variable from one data type to another.

  

### Syntax:

  

```php

$var = (int) "42";

$var = (float) "3.14";

$var = (string) 100;

$var = (bool) 1;

$var = (array) "text";

$var = (object) $array;

```

  

### Why use typecasting?

  

- To control data types explicitly

- For data validation and sanitization

- To avoid type juggling errors

- To ensure strict comparison works correctly

  

### Example:

  

```php

$price = "99.99";

$total = (float)$price * 2;

echo $total; // 199.98

```

  

---

  

## 2. Types of Typecasting in PHP

  

| Typecast      | Description                 | Example                |

|---------------|-----------------------------|------------------------|

| `(int)`       | Convert to integer           | `(int) "123.45" => 123`|

| `(float)`     | Convert to float/double      | `(float) "123" => 123.0`|

| `(string)`    | Convert to string            | `(string) 100 => "100"`|

| `(bool)`      | Convert to boolean           | `(bool) 0 => false`    |

| `(array)`     | Convert to array             | `(array) "hello" => ["hello"]` |

| `(object)`    | Convert to object            | `(object) ["name"=>"John"]`|

  

---

  

## 3. PHP Constants

  

A **constant** holds a fixed value that cannot change during script execution.

  

### Define constants:

  

```php

define("SITE_NAME", "MyWebsite");

const PI = 3.14159;

```

  

### Characteristics:

  

- No `$` prefix

- Global scope

- Value cannot be changed once set

  

### Example:

  

```php

define("DB_HOST", "localhost");

echo DB_HOST; // localhost

```

  

---

  

## 4. PHP Control Structures

  

Control structures control the flow of execution.

  

### Conditional statements:

  

```php

if ($age >= 18) {

    echo "Adult";

} else {

    echo "Minor";

}

  

switch ($day) {

    case "Monday":

        echo "Start of week";

        break;

    default:

        echo "Other day";

}

```

  

### Loops:

  

```php

for ($i = 1; $i <= 5; $i++) {

    echo $i;

}

  

while ($i <= 5) {

    echo $i++;

}

  

foreach ($arr as $value) {

    echo $value;

}

```

  

### Flow keywords:

  

- `break` — exit loop or switch

- `continue` — skip to next loop iteration

- `return` — exit from function

  

---

  

## 5. PHP Comparison Operators

  

| Operator | Meaning          | Example        | Result         |

|----------|------------------|----------------|----------------|

| `==`     | Equal            | `5 == "5"`     | `true`         |

| `===`    | Identical (value + type) | `5 === "5"` | `false`       |

| `!=`     | Not equal        | `5 != 4`       | `true`         |

| `<>`     | Not equal        | `5 <> 4`       | `true`         |

| `!==`    | Not identical    | `5 !== "5"`    | `true`         |

| `>`      | Greater than     | `7 > 5`        | `true`         |

| `<`      | Less than        | `7 < 5`        | `false`        |

| `>=`     | Greater or equal | `5 >= 5`       | `true`         |

| `<=`     | Less or equal    | `4 <= 5`       | `true`         |

  

---

  

## 6. PHP Logical Operators

  

| Operator | Meaning               | Example                   | Result       |

|----------|-----------------------|---------------------------|--------------|

| `&&`     | AND                   | `$x > 5 && $x < 10`       | true if both |

| `||`     | OR                    | `$x < 5 || $x > 10`       | true if one  |

| `!`      | NOT                   | `!($x == 5)`              | reverses     |

| `and`    | AND (low precedence)  | `true and false`          | false        |

| `or`     | OR (low precedence)   | `true or false`           | true         |

| `xor`    | Exclusive OR          | `true xor false`          | true if only one true |

  

---

  

## 7. `if...else` Statement in PHP

  

### Syntax:

  

```php

if (condition) {

    // code if true

} else {

    // code if false

}

```

  

### Example:

  

```php

$age = 20;

  

if ($age >= 18) {

    echo "You can vote.";

} else {

    echo "You cannot vote.";

}

```

  

### Extended version:

  

```php

if ($score >= 90) {

    echo "A";

} elseif ($score >= 75) {

    echo "B";

} else {

    echo "C";

}

```

  

---

  

## 8. PHP `switch` Statement

  

Used to select one of many blocks of code to be executed.

  

### Syntax:

  

```php

switch ($variable) {

    case value1:

        // code

        break;

    case value2:

        // code

        break;

    default:

        // code if no cases match

}

```

  

### Example:

  

```php

$day = "Tuesday";

  

switch ($day) {

    case "Monday":

        echo "Start of week";

        break;

    case "Tuesday":

        echo "Second day";

        break;

    default:

        echo "Other day";

}

```

  

---

  

If you want, I can also save this as a `.md` file and provide it for you! Just let me know.